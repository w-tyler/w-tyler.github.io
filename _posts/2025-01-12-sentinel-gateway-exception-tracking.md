---
layout: post
title: "Sentinel 网关集成：手动异常追踪实现熔断降级"
date: 2025-01-12
categories: [微服务, 熔断降级]
tags: [sentinel, gateway, 熔断, 降级, spring-cloud]
---

## 问题背景

Sentinel 支持网关的流控降级，在 Sentinel 的降级规则中有异常比例和异常数触发熔断降级的规则。但是如果 Sentinel 集成于 Gateway 之中，网关转发后的请求如果出现服务异常，Sentinel 并没有触发降级。

换而言之，**Sentinel 压根没有记录此次请求为异常请求**，因此，需要我们手动改造一下。

## 问题分析

大概读了一下 Sentinel 1.8.0 的源码，Sentinel 要记录异常需要 slot 插槽链中出现报错，因此网关转发后服务出现异常并不会被记录。

当请求经过网关转发到下游服务时：
1. Sentinel 只能监控到网关这一层的异常
2. 下游服务返回的 HTTP 错误状态码（如 500）对 Sentinel 来说是正常的响应
3. 这导致异常比例和异常数规则无法正常工作

## 解决方案

Sentinel 提供了手动埋点的方式，可以将自定义的插槽放入插槽链中。因此我们在 Gateway 中设置 Filter，拦截返回的请求，如果返回的 code 为 500 错误，则手动记录一次异常请求。

### 实现代码

```java
import org.reactivestreams.Publisher;
import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.cloud.gateway.filter.GlobalFilter;
import org.springframework.cloud.gateway.route.Route;
import org.springframework.cloud.gateway.support.ServerWebExchangeUtils;
import org.springframework.core.Ordered;
import org.springframework.core.io.buffer.DataBuffer;
import org.springframework.http.server.reactive.ServerHttpResponse;
import org.springframework.http.server.reactive.ServerHttpResponseDecorator;
import org.springframework.stereotype.Component;
import org.springframework.web.server.ServerWebExchange;

import com.alibaba.csp.sentinel.Entry;
import com.alibaba.csp.sentinel.EntryType;
import com.alibaba.csp.sentinel.SphU;
import com.alibaba.csp.sentinel.Tracer;
import com.alibaba.csp.sentinel.slots.block.BlockException;

import reactor.core.publisher.Mono;

@Component
public class SentinelStatusFilter implements GlobalFilter, Ordered {

    @Override
    public int getOrder() {
        // 设置优先级，需要小于 -1
        return -2;
    }

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {

        ServerHttpResponse originalResponse = exchange.getResponse();
        ServerHttpResponseDecorator decoratedResponse = new ServerHttpResponseDecorator(originalResponse) {

            @Override
            public Mono<Void> writeWith(Publisher<? extends DataBuffer> body) {
                // 获取此次请求命中的路由，以便记录异常到具体资源
                Route route = (Route) exchange.getAttributes()
                    .get(ServerWebExchangeUtils.GATEWAY_ROUTE_ATTR);
                String id = route.getId();
                
                // 返回500错误则记录异常，判断条件可自定义
                if (originalResponse.getRawStatusCode() == 500) {
                    Entry entry = null;
                    try {
                        // 第三个参数为0，这样返回的时候不会再次累加QPS数
                        entry = SphU.entry(id, EntryType.OUT, 0);
                        Tracer.trace(new Exception("Gateway downstream service error"));
                    } catch (BlockException e) {
                        e.printStackTrace();
                    } finally {
                        if (entry != null) {
                            entry.close();
                        }
                    }
                }
                return super.writeWith(body);
            }
        };
        
        // 使用装饰器替换原始响应
        return chain.filter(exchange.mutate().response(decoratedResponse).build());
    }
}
```

## 核心实现原理

### 1. GlobalFilter 拦截

通过实现 `GlobalFilter` 接口，我们可以拦截所有经过网关的请求和响应。设置 `getOrder()` 返回 -2，确保这个过滤器在 Sentinel 相关过滤器之后执行。

### 2. ResponseDecorator 装饰器模式

使用 `ServerHttpResponseDecorator` 装饰原始响应，在 `writeWith` 方法中拦截响应写入过程，此时可以获取到实际的 HTTP 状态码。

### 3. 手动异常记录

- 获取当前请求命中的路由 ID 作为资源名
- 当检测到 500 状态码时，通过 `SphU.entry()` 创建 Sentinel 入口
- 使用 `Tracer.trace()` 手动记录异常
- 第三个参数设置为 0，避免重复计算 QPS

## 扩展功能

### 支持更多错误状态码

```java
// 可以根据需要扩展错误状态码判断
if (originalResponse.getRawStatusCode() >= 500) {
    // 记录5xx错误
} else if (originalResponse.getRawStatusCode() == 404) {
    // 可选择是否记录404错误
}
```

### 基于响应内容判断

如果需要根据返回的数据进行更精确的判断，可以参考[这篇文章](https://www.cnblogs.com/commissar-Xia/p/11651196.html)来获取和解析响应体内容。

```java
// 示例：基于响应内容判断
if (isBusinessError(responseBody)) {
    // 记录业务异常
    Tracer.trace(new Exception("Business logic error"));
}
```

## 配置 Sentinel 规则

在 Sentinel 控制台配置相应的降级规则：

1. **异常比例规则**：当异常比例达到阈值时触发熔断
2. **异常数规则**：当异常数量达到阈值时触发熔断
3. **资源名称**：使用 Gateway 路由 ID 作为资源名

## 总结

通过这种手动埋点的方式，我们成功地让 Sentinel 能够感知到下游服务的异常，从而实现真正有效的熔断降级。这种方案的优势在于：

- **精确控制**：可以根据具体业务需求定义什么是"异常"
- **灵活配置**：支持多种错误判断条件
- **无侵入性**：不需要修改下游服务代码
- **统一管理**：通过 Sentinel 控制台统一管理熔断规则

需要注意的是，这种方案会增加一定的性能开销，建议在生产环境中做好性能测试和监控。 
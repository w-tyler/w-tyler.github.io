source "https://rubygems.org"

# 使用 GitHub Pages 官方支持的 Jekyll 版本
gem "github-pages", group: :jekyll_plugins

# 如果你需要本地开发，添加以下插件
group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-sitemap"
  gem "jekyll-seo-tag"
end

# Windows 和 JRuby 不包含 zoneinfo 文件，所以捆绑 tzinfo-data gem
# 并且关联到 x64-mingw32 和 java 平台
gem "tzinfo-data", platforms: [:mingw, :mswin, :x64_mingw, :jruby]

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin] 
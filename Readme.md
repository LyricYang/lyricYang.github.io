# YM Blog

基于 [Jekyll](https://jekyllrb.com/) 和 [GitHub Pages](https://pages.github.com/) 的个人博客项目。

## 目录结构

- `_posts/`：博客文章（Markdown 格式）
- `_layouts/`、`_includes/`：页面模板和组件
- `css/`、`js/`、`img/`：静态资源
- `Gemfile.txt`：Ruby 依赖管理
- `Gruntfile.js`：前端自动化任务
- `Rakefile.txt`：自动化脚本（如新建文章、预览）
- `_config.yml`：Jekyll 配置文件

## 本地开发

1. **安装 Ruby 环境**  
   推荐使用 [rbenv](https://github.com/rbenv/rbenv) 或 [rvm](https://rvm.io/) 管理 Ruby 版本。

2. **安装依赖**  
   ```sh
   gem install bundler
   bundle install --gemfile=Gemfile.txt
   ```

3. **编译前端资源（可选）**  
   如需开发样式或 JS，需先安装 Node.js，然后：
   ```sh
   npm install
   grunt           # 编译 less/压缩 js
   ```

4. **本地预览**  
   ```sh
   bundle exec jekyll serve
   ```
   或使用 Rake 脚本：
   ```sh
   rake preview
   ```

## 部署到 GitHub Pages

1. **推送到 GitHub 仓库**  
   只需将源码推送到 `master` 或 `main` 分支，GitHub Pages 会自动构建并发布。

2. **自定义域名**  
   在仓库根目录添加 `CNAME` 文件，内容为你的域名。

3. **自动化部署（可选）**  
   项目包含 `.travis.yml`，可用 [Travis CI](https://travis-ci.org/) 实现自动构建和部署。

## 进阶功能

- **PWA/离线支持**：见 [sw.js](sw.js) 和 [docs/Readme_sw.md](docs/Readme_sw.md)
- **RSS 订阅**：见 [feed.xml](feed.xml) 和 [docs/Readme_feed.md](docs/Readme_feed.md)
- **Grunt 自动化**：见 [Gruntfile.js](Gruntfile.js) 和 [docs/Readme_Gruntfile.md](docs/Readme_Gruntfile.md)
- **Rake 自动化**：见 [Rakefile.txt](Rakefile.txt) 和 [docs/Readme_Rakefile.md](docs/Readme_Rakefile.md)

## 参考

- [Jekyll 官方文档](https://jekyllrb.com/docs/)
- [GitHub Pages 官方文档](https://docs.github.com/en/pages)
- [Hux Blog](https://github.com/Huxpro/huxpro.github.io)（本项目灵感来源）
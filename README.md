# Browser Web Search Skill

把任何网站变成命令行 API，专为 OpenClaw 设计，复用浏览器登录态。

## 使用

无需安装，通过 npx 直接运行：

```bash
# 查看所有可用命令
npx --yes browser-web-search@^0.2.0 site list

# 运行 adapter
npx --yes browser-web-search@^0.2.0 zhihu/hot
npx --yes browser-web-search@^0.2.0 xiaohongshu/search "旅行"
npx --yes browser-web-search@^0.2.0 bilibili/popular

# JSON 输出 + jq 过滤
npx --yes browser-web-search@^0.2.0 zhihu/hot --json
npx --yes browser-web-search@^0.2.0 zhihu/hot --jq '.[].title'
```

## 内置平台

13 平台，41 个命令：

- **搜索**：Google, Baidu, Bing
- **社交**：小红书, 知乎
- **新闻**：36kr, 今日头条
- **开发**：GitHub, CSDN, 博客园
- **视频**：Bilibili
- **娱乐**：豆瓣
- **招聘**：BOSS直聘

## 前提条件

- Node.js >= 18.0.0
- OpenClaw 环境（`openclaw` 命令可用）

## 文档

详细使用说明请查看 [SKILL.md](./SKILL.md)

## 相关链接

- [browser-web-search](https://github.com/sipingme/browser-web-search) - 核心库
- [npm](https://www.npmjs.com/package/browser-web-search)

## License

MIT

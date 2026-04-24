---
name: browser-web-search
description: 一行命令搜遍全网 — 30 个平台 50 个命令，头条、知乎、GitHub、Reddit、Hacker News 等。专为 OpenClaw 设计，复用浏览器登录态，返回结构化 JSON，天然适配 AI Agent 工具调用。
version: 0.4.2
author: Ping Si <sipingme@gmail.com>
type: cli
requires:
  runtime:
    - name: node
      version: ">=18.0.0"
      description: Node.js 运行时
    - name: npm
      description: Node.js 包管理器（随 Node.js 安装）
  packages:
    - npm: browser-web-search
      global: false
  binaries:
    - name: openclaw
      description: OpenClaw CLI，用于浏览器自动化
install:
  command: npm install -g browser-web-search@0.4.2
  riskLevel: medium
  riskReason: 通过 npm 全局安装第三方包，该包会在浏览器页面上下文中执行 JavaScript。安装前请审计源码。
  requiresApproval: true
  source:
    registry: npmjs.com
    package: browser-web-search
    repository: https://github.com/sipingme/browser-web-search
    npm: https://www.npmjs.com/package/browser-web-search
  verification:
    - 安装前请审查 GitHub 仓库代码
    - 检查 npm 包的下载量和维护状态
    - 对比 npm 发布版本与 GitHub 源码是否一致
  note: 用户需先通过 npm install -g 全局安装 browser-web-search，运行时调用本地已安装的 bws 命令
capabilities:
  sensitive:
    - type: browser-session-access
      riskLevel: high
      description: 通过 OpenClaw 在已认证的浏览器标签页中执行 JavaScript
      scope: 按 adapter 域名隔离（如 zhihu.com, xiaohongshu.com）
      access:
        - 当前页面 DOM
        - 当前页面 Session（继承，非提取）
        - 站点认证数据（登录态下的 API 响应）
        - 账户保护页面内容（如私信、收藏、个人资料）
      noAccess:
        - 浏览器 Cookie 文件（不直接读取）
        - 其他域名数据
        - 用户配置目录
      risks:
        - 第三方 npm 包（browser-web-search）在页面上下文中执行，可访问站点认证数据
        - 恶意代码可能窃取 cookies 或页面内容
        - 包代码不包含在此 Skill 中，需独立审计
      mitigations:
        - adapter 脚本开源可审计
        - 按域名隔离，无法跨站访问
        - 不持久化存储任何凭证
  privacyNotice:
    summary: 此 Skill 自动复用浏览器登录态，可读取您已登录站点的任何可见数据
    details:
      - 零配置意味着 CLI 自动获得您在 OpenClaw 浏览器中的登录会话访问权
      - 可读取账户保护的页面（私信、收藏、个人资料、订单等）
      - 访问范围取决于您在目标站点的登录权限
      - 建议仅在信任 browser-web-search 包代码后使用
configPaths:
  - path: ~/.bws/
    required: false
tags:
  - browser
  - web-search
  - scraping
  - automation
  - ai-agent
repository: https://github.com/sipingme/browser-web-search-skill
package: https://github.com/sipingme/browser-web-search
npm: https://www.npmjs.com/package/browser-web-search
---

# Browser Web Search (BWS) Skill

> **一行命令，搜遍全网** — 为 AI Agent 而生的多平台内容搜索工具

把 30 个主流平台的搜索接口封装成统一命令行 API，让 AI Agent 直接拿到结构化 JSON，无需 API Key，无需额外配置。

## 🏗️ 架构说明

```
OpenClaw/AI Agent
    ↓ (读取 Skill 配置)
browser-web-search-skill
    ↓ (调用 CLI)
bws 命令
    ↓ (OpenClaw Browser)
目标网站（30 个平台）
```

## 🎯 核心特点

- 🔍 **跨平台搜索** — 今日头条、知乎、GitHub、Reddit、Hacker News… 一套语法搞定
- 🔑 **无需 API Key** — 复用浏览器登录态，开箱即用
- 🤖 **AI Agent 友好** — 结构化 JSON 输出，支持 `--jq` 过滤，天然适配 LLM 工具调用
- ⚡ **零配置** — 无需 Chrome Extension，无需后台 Daemon

## 📋 安装

```bash
npm install -g browser-web-search@0.4.2
```

### 验证安装

```bash
bws --version
bws site list
```

## 🚀 快速开始

```bash
# 搜索今日头条关于 "ai search" 的最新文章
bws site toutiao/search "ai search"

# 搜索知乎，返回 5 条
bws site zhihu/search "ai agent" --count 5

# Hacker News 最新讨论（按时间）
bws site hn/search "llm" --sort date

# GitHub 热门仓库（按 Star 数）
bws site github/search "ai search" --sort stars

# Reddit 最新帖子
bws site reddit/search "ai search" --sort new

# 查看所有可用命令
bws site list
```

## 📊 内置平台（30 个）

### 🇨🇳 国内平台（20 个）

| 平台 | 说明 | 命令 |
|-----|------|-----|
| **今日头条** | 新闻资讯 | `toutiao/search`, `toutiao/hot`, `toutiao/feed` |
| **微信公众号** | 公众号文章 | `weixin/search`, `weixin/article` |
| **小红书** | 生活分享 | `xiaohongshu/search`, `xiaohongshu/note` |
| **知乎** | 问答社区 | `zhihu/search`, `zhihu/hot`, `zhihu/question`, `zhihu/me` |
| **微博** | 社交热搜 | `weibo/search`, `weibo/hot` |
| **Bilibili** | 视频弹幕 | `bilibili/search`, `bilibili/popular`, `bilibili/trending`, `bilibili/ranking`, `bilibili/video` |
| **澎湃新闻** | 权威新闻 | `thepaper/search`, `thepaper/hot` |
| **腾讯新闻** | 热点新闻 | `qqnews/search`, `qqnews/hot` |
| **网易新闻** | 热点新闻 | `netease/search`, `netease/hot` |
| **新浪新闻** | 门户新闻 | `sina/search`, `sina/hot` |
| **36kr** | 科技创投 | `36kr/search`, `36kr/newsflash`, `36kr/article` |
| **虎嗅** | 科技商业媒体 | `huxiu/search` |
| **华尔街见闻** | 财经资讯 | `wallstreetcn/search` |
| **雪球** | 股票社区 | `xueqiu/search` |
| **掘金** | 技术社区 | `juejin/search` |
| **CSDN** | 开发者社区 | `csdn/search` |
| **博客园** | 技术博客 | `cnblogs/search` |
| **V2EX** | 技术社区 | `v2ex/search` |
| **BOSS直聘** | 招聘平台 | `boss/search`, `boss/detail` |
| **Baidu** | 百度搜索 | `baidu/search` |

### 🌏 国际平台（10 个）

| 平台 | 说明 | 命令 |
|-----|------|-----|
| **Google** | 谷歌搜索 | `google/search` |
| **Bing** | 必应搜索 | `bing/search` |
| **GitHub** | 代码托管 | `github/search` |
| **Hacker News** | 科技社区 (YC) | `hn/search` |
| **Reddit** | 英文社区 | `reddit/search` |
| **X (Twitter)** | 社交媒体 | `x/search` |
| **The Verge** | 科技媒体 | `verge/search` |
| **Ars Technica** | 深度科技媒体 | `ars/search` |
| **Engadget** | 科技消费媒体 | `engadget/search` |
| **InfoQ** | 技术媒体 | `infoq/search` |

## 🔧 命令参考

```bash
bws site list                        # 列出所有 adapter
bws site info <name>                 # 查看 adapter 参数说明
bws site <name> [args...]            # 运行 adapter
bws site <name> --count 5           # 限制返回数量
bws site <name> --json               # 输出原始 JSON
bws site <name> --jq '.items[].url' # jq 过滤提取字段
```

## 📋 标准操作流程 (SOP)

### 操作 1：跨平台搜索

**场景**：用户想搜索多个平台关于某话题的最新内容

```bash
# 国内平台
bws site toutiao/search "ai agent" --count 5
bws site zhihu/search "ai agent" --count 5
bws site huxiu/search "ai agent" --count 5

# 国际平台
bws site hn/search "ai agent" --sort date --count 5
bws site reddit/search "ai agent" --sort new --count 5
bws site github/search "ai agent" --sort stars --count 5
```

---

### 操作 2：获取热点资讯

```bash
bws site zhihu/hot        # 知乎热榜
bws site weibo/hot        # 微博热搜
bws site toutiao/hot      # 今日头条热榜
bws site thepaper/hot     # 澎湃新闻热点
```

---

### 操作 3：使用 jq 过滤数据

```bash
# 只提取标题
bws site zhihu/search "大模型" --jq '[.items[].title]'

# 只提取 URL 列表
bws site hn/search "llm" --jq '[.items[].url]'

# 提取标题+日期
bws site toutiao/search "ai" --jq '[.items[] | {title, date}]'
```

---

### 操作 4：搜索文章并获取完整正文（配合 news-to-markdown）

**两步流水线**：

```bash
# Step 1：用 bws 搜索，拿到文章 URL 列表
bws site toutiao/search "ai agent" --count 3

# Step 2：对每个 url 调用 news-to-markdown 获取正文
npx --yes news-to-markdown@latest "https://www.toutiao.com/article/xxx"
```

**适用平台**：头条、微信公众号、36kr、知乎、小红书等 news-to-markdown 支持的平台

---

### 操作 5：登录态管理

部分平台（微信公众号、小红书、微博、X 等）需要登录：

```bash
# 在 OpenClaw 浏览器中登录
openclaw browser open https://weixin.qq.com

# 登录完成后重试
bws site weixin/search "ai"
```

---

## 🔧 技术架构：如何访问登录态

```
bws 命令
    ↓ 调用
openclaw browser evaluate <script>
    ↓ 在已打开的标签页中执行 JavaScript
目标网站（使用该标签页的登录态）
```

| 访问内容 | 是否访问 | 说明 |
|---------|---------|------|
| 浏览器 Cookie 文件 | ❌ 否 | 不直接读取 `~/.config/chromium/Cookies` 等文件 |
| 用户配置目录 | ❌ 否 | 不访问 `~/.bws/` 以外的配置 |
| 其他网站数据 | ❌ 否 | 只能访问 adapter 指定的域名 |
| 当前页面 DOM | ✅ 是 | adapter 脚本在页面中执行 |
| 当前页面 Session | ✅ 是 | 继承页面的登录状态 |

## 🔒 安全性说明

- ✅ 所有操作在本地执行
- ✅ 按域名隔离，无法跨站访问
- ❌ 不会收集用户信息
- ❌ 不会上传到第三方服务器

## 🎓 示例对话

**用户**：搜索头条最新 3 篇关于 AI Agent 的文章

```bash
bws site toutiao/search "AI Agent" --count 3
```

---

**用户**：看看 Hacker News 上关于 LLM 的最新讨论

```bash
bws site hn/search "llm" --sort date --count 5
```

---

**用户**：GitHub 上 ai search 相关的热门项目

```bash
bws site github/search "ai search" --sort stars --count 5
```

---

## 📚 参考资料

- [项目 GitHub](https://github.com/sipingme/browser-web-search-skill)
- [browser-web-search 核心库](https://github.com/sipingme/browser-web-search)
- [npm 包](https://www.npmjs.com/package/browser-web-search)

---

## 📝 维护说明

- **版本**: 0.4.2
- **最后更新**: 2026-04-24
- **维护者**: Ping Si <sipingme@gmail.com>
- **许可证**: MIT

---

## ✅ 首次成功检查清单

- [ ] 安装工具：`npm install -g browser-web-search@0.4.2`
- [ ] 验证版本：`bws --version`
- [ ] 查看命令：`bws site list`
- [ ] 测试搜索：`bws site zhihu/search "ai" --count 3`
- [ ] 看到 JSON 输出

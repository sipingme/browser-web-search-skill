# Browser Web Search Skill

> **一行命令，搜遍全网** — 30 个平台 50 个命令，专为 OpenClaw 与 AI Agent 设计

## 快速开始

```bash
# 安装
npm install -g browser-web-search@0.4.2

# 查看所有命令
bws site list

# 搜索示例
bws site toutiao/search "ai search"       # 今日头条
bws site zhihu/search "ai agent" --count 5  # 知乎
bws site hn/search "llm" --sort date      # Hacker News
bws site github/search "ai search" --sort stars  # GitHub

# jq 过滤
bws site zhihu/search "ai" --jq '[.items[].url]'
```

## 内置平台（30 个）

### 🇨🇳 国内平台（20 个）

| 平台 | 命令 |
|-----|------|
| **今日头条** | `toutiao/search`, `toutiao/hot`, `toutiao/feed` |
| **微信公众号** | `weixin/search`, `weixin/article` |
| **小红书** | `xiaohongshu/search`, `xiaohongshu/note` |
| **知乎** | `zhihu/search`, `zhihu/hot`, `zhihu/question`, `zhihu/me` |
| **微博** | `weibo/search`, `weibo/hot` |
| **Bilibili** | `bilibili/search`, `bilibili/popular`, `bilibili/trending`, `bilibili/ranking`, `bilibili/video` |
| **澎湃新闻** | `thepaper/search`, `thepaper/hot` |
| **腾讯新闻** | `qqnews/search`, `qqnews/hot` |
| **网易新闻** | `netease/search`, `netease/hot` |
| **新浪新闻** | `sina/search`, `sina/hot` |
| **36kr** | `36kr/search`, `36kr/newsflash`, `36kr/article` |
| **虎嗅** | `huxiu/search` |
| **华尔街见闻** | `wallstreetcn/search` |
| **雪球** | `xueqiu/search` |
| **掘金** | `juejin/search` |
| **CSDN** | `csdn/search` |
| **博客园** | `cnblogs/search` |
| **V2EX** | `v2ex/search` |
| **BOSS直聘** | `boss/search`, `boss/detail` |
| **Baidu** | `baidu/search` |

### 🌏 国际平台（10 个）

| 平台 | 命令 |
|-----|------|
| **Google** | `google/search` |
| **Bing** | `bing/search` |
| **GitHub** | `github/search` |
| **Hacker News** | `hn/search` |
| **Reddit** | `reddit/search` |
| **X (Twitter)** | `x/search` |
| **The Verge** | `verge/search` |
| **Ars Technica** | `ars/search` |
| **Engadget** | `engadget/search` |
| **InfoQ** | `infoq/search` |

## 环境要求

- Node.js >= 18.0.0
- OpenClaw 环境

## 文档

详细使用说明：[SKILL.md](./SKILL.md)

## 链接

- [GitHub](https://github.com/sipingme/browser-web-search)
- [npm](https://www.npmjs.com/package/browser-web-search)

## License

MIT

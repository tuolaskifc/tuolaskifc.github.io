# 网站说明文档 · tuolaskifc.github.io

> **维护规则：每次对网站做任何改动（新增/删除/修改页面、交互、样式、配置），都必须同步更新本文件。**
> 本文件是"总览 + 索引"，彩蛋细节见 `EASTER_EGG_DESIGN.md`（化学人格鉴定）和 `PLAY_PAGE_DESIGN.md`（小游戏合集）。
> 本文件已加入 `_config.yml` 的 exclude，不会部署上线。

---

## 一、网站概览

| 项 | 说明 |
|----|------|
| 线上地址 | https://tuolaskifc.github.io |
| 站长 | 刘阳（Liu Yang）· 清华大学 · 致理书院 · 化学大二 |
| 定位 | 学术名片（外层）+ 学习记录 + 私人游乐场（内层，彩蛋触发） |
| 主题 | APEX Legends 风格（暗黑 #0a0a0a / 主红 #8b1a1a / 沙色 #c9b99a） |
| 技术栈 | Jekyll（静态站）+ GitHub Pages legacy 自动构建 |
| 语言 | 中文（默认 `/`）+ 英文（`/en/`）双版 |

---

## 二、技术栈与构建

- **Jekyll 静态网站生成器**，源码在 `personal-website/` 目录
- 推送到 GitHub `main` 分支 → **GitHub Pages legacy 构建自动部署**（已删 Actions workflow，避免双构建冲突）
- 配置在 `_config.yml`：站点信息、exclude 列表、默认布局
- **本机没有 Ruby/Jekyll**，无法本地预览，改完直接 push 看线上
- 无 Gemfile，纯 GitHub Pages 标准构建

---

## 三、文件 / 目录结构

```
personal-website/
├── _config.yml             # 站点配置（标题/描述/URL/分页/exclude）
├── index.html              # 中文首页（layout: default，lang: zh）
├── index-en.html           # 英文首页（permalink: /en/，lang: en）
├── blog/index.html         # 博客列表页（分页）
├── easter-egg.html         # 彩蛋①：化学人格鉴定（permalink: /easter-egg/）
├── easter-egg-apex.html    # 彩蛋②：B站视频合集（permalink: /easter-egg-apex/）
├── easter-egg-play.html    # 彩蛋③：小游戏合集（permalink: /play/）
├── _layouts/
│   ├── default.html        # 主布局：splash + nav + 全部彩蛋触发 + 主题/语言
│   └── post.html           # 文章页布局（继承 default，支持 course 课程徽章）
├── _posts/                 # 博客文章（文件名 = 日期-标题，front matter 控制可见性）
│   ├── 2026-06-12-umpolung.md                          # 公开 · 有机化学H1研讨课（course 徽章）
│   ├── 2026-07-05-welcome.md                           # 公开
│   ├── 2026-07-20-ai4s-compound-c-nmr-analysis.md      # 公开 · AI4S交叉实践课（course 徽章）
│   └── 2026-08-26-browser-plugins.md                   # 公开 · 咪咕/爱奇艺防剧透插件分享
├── assets/
│   ├── css/style.css       # 全站样式（主题变量、splash、卡片、彩蛋样式）
│   └── images/
│       ├── og-card.png     # Open Graph 分享卡片图（1200×630）
│       ├── ai4s-compound-c/  # NMR 博客配图（slide-00 ~ slide-19）
│       └── umpolung/         # 极性反转博客配图（slide-00 ~ slide-16）
├── EASTER_EGG_DESIGN.md    # 彩蛋① 设计文档（exclude，不上线）
├── PLAY_PAGE_DESIGN.md     # 彩蛋③ 设计文档（exclude，不上线）
└── WEBSITE_GUIDE.md        # 本文件（exclude，不上线）
```

---

## 四、页面清单

| URL | 文件 | 布局 | 可见性 | 说明 |
|-----|------|------|--------|------|
| `/` | index.html | default | 公开 | 中文首页：hero + 兴趣与展望 + 最新文章 + 关于 |
| `/en/` | index-en.html | default | 公开 | 英文首页（内容英文化） |
| `/blog/` | blog/index.html | default | 公开 | 博客列表（`site.posts` 全量列出），私密文章默认隐藏 |
| `/blog/:year/:month/:day/:title/` | _posts/*.md | post | 按 front matter | 文章页 |
| `/easter-egg/` | easter-egg.html | 无(null) | 彩蛋 | 化学人格鉴定游戏 |
| `/easter-egg-apex/` | easter-egg-apex.html | 无(null) | 彩蛋 | B站视频合集 |
| `/play/` | easter-egg-play.html | 无(null) | 彩蛋 | 小游戏合集（含化学老虎机） |
| 任意页面 | — | default | — | 均有 splash（仅首页）+ nav + footer + 彩蛋触发 |

---

## 五、可交互元素总表（核心章节）

### 5.1 Splash 入场屏（仅首页 `/` 和 `/en/`）

| 交互 | 触发 | 效果 |
|------|------|------|
| 进入首页 | 自动 | 显示 APEX 风格入场动画（MIEMIE logo + 继续按钮） |
| 继续 / 点击空白 / 按 ESC | 点击/按键 | 淡出进入网站 |
| 会话记忆 | 自动 | `sessionStorage['splashEntered']`：同一次会话只显示一次，点过导航回来不再重复 |
| 主题切换 | 点击 🌙/☀️ | 切换深/浅色，写入 `localStorage['theme']` |
| 语言切换 | 点击 EN/中 | 跳转 `/en/` 或 `/`（splash 按钮文案随语言变） |

### 5.2 顶部导航（所有 default 布局页面）

| 交互 | 触发 | 效果 |
|------|------|------|
| 品牌名 | 点击 | 回首页 `/` |
| 首页/兴趣与展望/博客/关于 | 点击 | 跳转（英文版显示 Home/Interests/Blog/About） |
| 主题切换 🌙/☀️ | 点击 | 全局深/浅色切换，记忆在 localStorage |
| 语言切换 EN/中 | 点击 | 中英文首页互跳 |
| 站长徽章 🔓站长 | 仅站长模式 | 显示在 nav 右侧，普通访客看不到 |

### 5.3 站长模式（Konami Code）

- **触发**：在任意 default 页面输入 `↑ ↑ ↓ ↓ ← → ← → B A`
- **效果**：
  - 显示/隐藏所有私密文章（列表里的 🔒 文章）
  - nav 出现 "🔓 站长" 徽章
  - 弹出 toast 提示"站长模式已开启/关闭"
- **状态记忆**：`sessionStorage['adminMode']`，本次会话内有效
- **再次输入同一指令** = 关闭

### 5.4 彩蛋触发词（键盘输入，任意 default 页面）

| 指令 | 跳转 | 说明 |
|------|------|------|
| `ilovechen` | `/easter-egg/` | 化学人格鉴定（6 题测元素人格） |
| `apexlegend` | `/easter-egg-apex/` | B站视频合集（4 个嵌入视频） |
| `play` | `/play/` | 小游戏合集（当前 1 个游戏） |

- 输入有缓存：连打多个字母，命中即跳转
- 检测顺序：Konami → ilovechen → apexlegend → play（互不冲突）
- ✅ **全站生效**：所有页面（含文章页）都用 `layout: default` 或其派生布局

### 5.5 手机端彩蛋入口

- **触发**：在**页脚版权文字**上快速连点 **5 次**
- **效果**：跳转 `/easter-egg/`（化学人格鉴定）
- 每次点击间隔超过 1.2 秒计数清零
- ⚠️ 只进化学鉴定，不进视频/游戏页

### 5.6 私密文章系统

- 文章 front matter 加 `private: true` 即标记为私密
- **访客视角**：列表页卡片直接隐藏（CSS `display:none` + `data-private` 属性）
- **站长模式**：显示 🔒 私密卡片，可点击阅读
- **分享保护**：私密文章的 Open Graph 描述被替换为"🔒 私密内容"，不泄露正文摘要
- ⚠️ **本质是"前端隐藏"**：文章 URL 直达仍可看，源码可被查看。真正的隐私内容不要放 GitHub Pages（仓库是 public 的）。适合放"不想太显眼但不怕被看到"的内容

### 5.7 Open Graph / 分享卡片

- 所有 default 布局页面头部都有 og:image / og:title / og:description / og:url / canonical / twitter card
- 分享图：`assets/images/og-card.png`（APEX 风格，用 ImageMagick 生成）
- 微信/B站/QQ 转发时显示 APEX 预览卡片

### 5.8 博客相关

- 列表页卡片**整张可点**（含私密卡片，站长模式下）
- 文章页有 MathJax（front matter `mathjax: true`）
- 分页：每页 10 篇，上/下一页

---

## 六、彩蛋页内部交互

### 6.1 化学人格鉴定 `/easter-egg/`（细节见 EASTER_EGG_DESIGN.md）
- 全屏入场特效（MIEMIE + ACCESS GRANTED，2s）
- 介绍屏 → 开始鉴定 → 6 题（每题 4 选项）→ 结果屏
- 结果屏：重新鉴定 / 复制结果（clipboard，含分享文案）
- 27 个元素/化合物池，选项暗中加分，最高分胜出

### 6.2 B站视频 `/easter-egg-apex/`
- 4 个 B站 iframe 嵌入视频（可全屏播放）
- 无入场特效，直接展示

### 6.3 小游戏合集 `/play/`（细节见 PLAY_PAGE_DESIGN.md）
- 全屏入场特效（LABORATORY，2s）
- 游戏列表屏 → 选择游戏（当前仅"化学老虎机"）
- **化学老虎机**：
  - SPIN 按钮拉杆，3 列转盘（18 个化学符号，按稀有度加权）
  - 转动时 blur 残影，逐列停靠（400/800/1200ms）
  - 判定：三连传说/三连相同/两连/全稀有 → 加分
  - 计分：连击（streak）+ 最高分（best），**刷新即清零**（未持久化）
  - 返回游戏列表按钮

---

## 七、主题与样式体系

- 主题变量在 `assets/css/style.css` 顶部 `:root`（暗色）+ `.light` 覆盖（亮色）
- **克制红黑 + 白灰**配色：画布 `--bg:#0a0a0a`，主红 `--accent:#e04438`（配 `--accent-deep`/`--accent-grad` 组成一族），白灰高光 `--bright:#dcdce2`；背景灰阶逐层拉开（bg → card → tag），文字五级灰（text → text-light）
- splash 入场屏用固定色（浅 `#e8e6e6` → 红渐变），不随主题变量变，保证深色 logo 始终可见
- 默认跟随系统深浅色，用户手动切换后记忆在 `localStorage['theme']`
- 字体：Anton（标题）/ Inter / Noto Sans SC / Fira Code
- 彩蛋页自带独立 `<style>` 块，但色值已与主站统一（红族/白灰），不依赖主 style.css 变量也能协调

---

## 八、如何新增内容（快速指南）

### 新增一篇博客
```yaml
---
title: "文章标题"
date: 2026-08-08
tags: [标签1, 标签2]
course: 某课程/来源      # 可选：文章页会显示 🎓 课程徽章
private: true        # 想公开就删掉这一行
mathjax: true        # 需要公式就加上
---
正文内容...
```
- 文件名：`_posts/2026-08-08-slug.md`（日期必须等于 front matter 的 date）
- 配图放 `assets/images/某文件夹/`，正文用 `![]({{ site.baseurl }}/assets/images/... )` 引用

### 新增一个彩蛋/游戏
见 `PLAY_PAGE_DESIGN.md` 第五节"如何修改/添加"——加游戏卡片 + 面板 + 切换 JS。

### 修改彩蛋触发词
改 `_layouts/default.html` 里对应的数组（KONAMI / EASTER / APEX / PLAY），每个字符一个数组元素、小写。

---

## 九、已知问题 / 注意点

1. ~~文章页（post.html）功能缺失：文章页用独立布局，没有彩蛋触发、站长模式、语言切换、OG 分享卡片~~ ✅ **已修复（2026-08-08）**：post.html 改为继承 default 布局，文章页与主页功能完全一致
2. **私密文章是"前端隐藏"**：URL 直达 + 源码可看，不是真加密
3. **手机端彩蛋**只进化学鉴定，进不了视频/游戏页
4. **老虎机最高分**刷新清零，未做 localStorage 持久化
5. **本机无 Jekyll**，无法本地构建验证，改完直接 push 看线上效果
6. **GitHub 推送偶发网络失败**：多试几次即可（本机无代理时常见）

---

## 十、更新日志

| 日期 | 改动 |
|------|------|
| 2026-08-08 | 本文件创建。记录当前网站全部结构。此前已有：play 彩蛋页、OG 分享卡片、英文版首页 |
| 2026-08-08 | `post.html` 改为继承 `default` 布局：文章页获得彩蛋触发、站长模式、语言切换、OG meta，与主页功能一致（修复已知问题 1） |
| 2026-08-08 | 修复 splash 在非首页页面误显示：`.splash` CSS 中重复的 `display:flex` 覆盖了 `display:none`，删除后 splash 仅首页显示 |
| 2026-08-08 | 博客列表页改用 `site.posts`（`paginator` 在子目录不生效），删除 `_config.yml` 中分页配置；添加试管 SVG favicon + ICO 备用 |
| 2026-08-23 | 配色升级（方案 A·APEX 战魂，参考 Raycast 设计系统）：纯黑 `#08070a`、艳红 `#b52b2b`、沙金提亮 `#d8c9a3`；hero 顶部加红色斜条纹；OG 卡片与 favicon 配色同步 |
| 2026-09-03 | 部署方式统一：删除 `jekyll.yml` Actions workflow，改用 GitHub Pages legacy 构建（原 workflow 因"下划线目录"误判而多余，与 legacy 双构建冲突） |
| 2026-09-03 | 首页板块「研究兴趣」→「兴趣与展望」：3 张高深卡片改为 2 块务实内容（出国深造 / 用计算与 AI 解决化学），hero 简介同步收敛；板块锚点 `#research` → `#interests`，导航文案同步；卡片 grid 支持自动两列 |
| 2026-09-03 | 技能标签更新（中英同步）：去 RDKit/AI波谱，加 C/Python/机器学习/Vibe Coding/计算化学；新增文章页 `course` 字段 → 显示 🎓 课程徽章；极性反转文章转公开并改期 6-12（标注有机化学H1研讨课）；NMR 文章标注 AI4S 交叉实践课；新增插件分享文章（8-26） |
| 2026-09-03 | 配色系统重构（克制红黑 + 白灰）：删沙金 `--sand` → 新 `--bright` 白灰高光；红收敛为单族 `--accent`/`--accent-deep`/`--accent-grad`；背景灰阶拉开（bg 0a0a0a → card 16161a → tag 1d1d22）；hero 斜纹/splash/OG卡/favicon/彩蛋页全部同步新色 |

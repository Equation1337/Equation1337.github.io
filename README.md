# 个人主页 + 澜默档案 + 动态

一个零依赖的静态个人网站：主页作为外壳，**澜默档案**与**动态/帖子**作为其中的子模块。
无构建步骤、无框架、无外部服务，直接部署即可。

```
.
├─ index.html          主页（hero / 模块导航 / 关于 / 方向 / 链接）
├─ lanmo.html          澜默 · 银灰轶影 档案（子模块：自我介绍）
├─ posts.html          动态 / 帖子（子模块：可点赞、可评论、可发布）
├─ assets/
│   ├─ README.md       立绘投放规范（怎么把图接进来）
│   ├─ portrait.png    ← 普通形态立绘
│   ├─ portrait-ego.png← 神备形态立绘（可选）
│   ├─ sword.png       ← 长剑形态立绘（可选）
│   ├─ sword-gun.png   ← 枪械形态立绘（可选）
│   └─ avatar.png      ← 主页头像（可选）
└─ README.md           本文件
```

## 打开

双击 `index.html`。或用本地服务器（推荐，`?form=` 之类查询参数在 file:// 下也正常）：

```bash
python -m http.server 8080
# http://127.0.0.1:8080
```

## 怎么提供立绘（重点）

**把图片文件放进 `assets/` 文件夹，用固定文件名即可，无需改代码：**

| 文件名 | 用途 |
| --- | --- |
| `assets/portrait.png` | 普通形态立绘（疤 + 三角巾） |
| `assets/portrait-ego.png` | 神备形态立绘（可选；只放一张则两形态共用） |
| `assets/sword.png` | 长剑形态立绘（可选，横幅最好） |
| `assets/sword-gun.png` | 枪械形态立绘（可选；只放一张则两形态共用） |
| `assets/avatar.png` | 主页头像（可选，1:1） |

细节与推荐规格见 [assets/README.md](assets/README.md)。三种交付方式：

1. **放文件**（推荐）：拷进 `assets/` → 告诉我 → 我渲染确认。
2. **贴进对话**：直接贴图，我找附件落盘位置、复制进 `assets/` 并按规范命名。
3. **分层图**（PSD 等）：可做成「切换形态时三角巾层单独淡出」的叠加效果。

> 不放任何图也能看：页面会回退到内置的手绘 SVG 立绘。

## 主页需要你改的地方（都标了注释）

`index.html` 里凡是 `<!-- ▼ ... -->` 注释处，以及正文中黄色虚线的
<span style="color:#c2a468">占位</span> 文字，都是待你替换的：

- `<title>` 站点标题
- 顶部标识、`h1` 名字、拼音/ID
- 模块卡片的文字与链接（关于 / 方向 / 项目 / 联系；澜默、动态、链接已接好）
- 关于我正文与右侧信息表（`NAME / FOCUS / STAGE / ...`）
- **链接区**：`#links` 里「友情链接」2 条 + 「个人链接」4 条，把 `href` 换成真实网址、文字换掉即可
- 页脚版权

当前「关于我」内容是我根据你机器上的线索写的**初稿**（算法与数据结构、C++、Minecraft 模组、
Blender / Photoshop、Project Moon），请按实际情况删改，不必照单全收。

## 澜默档案（lanmo.html）的交互

- **形态切换（普通 ⇄ 神备）**：切到神备后，三角巾与疤痕一同淡出，脸颊恢复完好、双眼透出微光、
  身周浮起银蓝光环；蓝水晶项链始终保留。状态同步全站立绘。
  若换成你自己的图，页面会在图之上叠加一层「边缘强、中心透」的银蓝辉光，保证切换可见。
- **遗剑变形（长剑 ⇄ 枪械）**：演示可变形武装；放入 `sword*.png` 后会替换为你的剑立绘。
- **URL 直达**：`lanmo.html?form=ego`（神备）、`lanmo.html?arm=gun`（枪形态）。
  点按钮时地址栏也会自动同步，方便复制分享。

## 动态 / 帖子（posts.html）

页面顶部有一个 **「评论可见性」开关**，两种模式随时切：

| 模式 | 评论/点赞存在哪 | 别人能看到吗 | 需要配置吗 |
| --- | --- | --- | --- |
| **仅本机可见**（默认）| 浏览器 localStorage | ❌ 看不到 | 不需要，开箱即用 |
| **所有人可见** | 你的 GitHub Discussions（经 Giscus）| ✅ 能看到 | 需填 4 个值（见下）|

切换后：点赞按钮在本地模式下是「赞」（本机计数），在公开模式下变成「GitHub 反应」，
打开讨论串用 GitHub reaction 点赞；评论数会自动读取讨论串的真实评论数。
模式选择记在 localStorage，刷新后保持。

### 接通「所有人可见」（已为你配置完成 ✅）

**已经接好了，无需再操作。** 具体配置如下，供你日后参考或迁移：

| 项目 | 值 |
| --- | --- |
| 线上地址 | <https://equation1337.github.io/> |
| 评论存储仓库 | <https://github.com/Equation1337/site-comments>（公开） |
| Discussions 分类 | `Announcements` |
| 已建讨论串 | `lm-post-first-post`(#1)、`lm-post-wheat-mod`(#2)、`lm-post-algo-note`(#3) |
| Giscus App | 已安装，仅授权 `site-comments` 仓库 |

`posts.html` 里已填好的 4 个值：

```js
var GISCUS = {
  repo:       "Equation1337/site-comments",
  repoId:     "R_kgDOU9ZmFA",
  category:   "Announcements",
  categoryId: "DIC_kwDOU9ZmFM4DHIaT",
  reactions:  "1",          // 1 = 开 reaction（点赞即 GitHub reaction）
  lang:       "zh-CN",
  theme:      "dark_dimmed",
  inputPosition: "top"
};
```

**如果要换成别的仓库**（比如想用你自己的另一个仓库）：

1. 建一个 **公开** GitHub 仓库；该仓库 **Settings → General → Features → 勾选 Discussions**。
2. 安装 Giscus App：<https://github.com/apps/giscus> ，授权给它。
3. 打开 <https://giscus.app> 填仓库名，拿 4 个值，替换 `posts.html` 里的 `GISCUS` 配置。

**重要：Giscus 需要 `http(s)` 环境。** 线上（GitHub Pages）正常；
本地预览用 `python -m http.server 8080` 后访问 `http://127.0.0.1:8080/posts.html`；
**双击 `file://` 打开时不生效**（此时展开评论会显示操作指引而不是空白）。

### 新增帖子后，讨论串会自动创建

Giscus 会在访客第一次展开某帖评论、或你首次进入该帖讨论区时**自动创建**对应的 GitHub 讨论串；
我预先建好了现有 3 条帖的讨论串，所以可直接打开。新帖无需手动建。

### 每篇帖子 = 一个独立讨论串

脚本用 `data-term="lm-post-<data-id>"` 给每篇帖子映射一个 GitHub 讨论串。
所以发帖时 **`data-id` 要唯一**；改了 id，该帖在 GitHub 上的讨论串也会换新的。

### 怎么发帖（推荐：改源码）

在 `posts.html` 的 `<div class="feed" id="feed">` 里复制一个 `<article class="post">` 块：

```html
<article class="post rv" data-id="my-unique-id" data-date="2026-10-06" data-tags="标签A,标签B">
  <div class="pmeta">
    <span class="ttl">标题</span>
    <span class="tag">标签A</span>
    <span class="tag">标签B</span>
    <span class="date">2026-10-06</span>
  </div>
  <div class="pbody">
    <p>正文第一段</p>
    <p>第二段，链接：<a href="https://example.com">示例</a></p>
  </div>
</article>
```

保存刷新即可。**改动源码的帖子在所有访客那里都可见**（公开模式下评论也共享）。

页面底部那个「我要发一条」表单，是写进浏览器 localStorage 的临时记录，
永远只在你自己设备可见 —— 它方便你随手记，但**不是**公开发帖入口。

## 技术说明

- 单文件/零依赖：每页自带样式与脚本；无构建步骤、无框架。
- 字体走 Google Fonts CDN，离线自动回退到系统中文字体。
- 立绘为手写 SVG（`<symbol>` + `<use>` 复用），形态差异由 CSS 变量驱动。
- 落图用 `<img onerror>` 探测：**文件存在就显示，不存在就自动隐藏并回退 SVG**，所以你可以随时放图/删图。
- 响应式：1440 / 980 / 760 / 430 断点；`prefers-reduced-motion` 下关闭动效。

## 部署

纯静态，直接把整个文件夹传到任意静态托管（GitHub Pages / Vercel / Netlify / 对象存储）即可。
`index.html` 为入口；`lanmo.html`、`posts.html`、`assets/` 保持同级。

> 注意：`posts.html` 的「所有人可见」评论依赖 Giscus，需要 `http(s)` 环境。
> 所以**用了公开评论就务必部署**（或用本地服务器预览），不要只靠双击 `file://` 打开。
> 其余所有功能（含本机模式评论、澜默档案的形态切换）在 `file://` 下都能正常工作。


# 个人IP网站导航与项目展示 — 设计文档

**日期:** 2026-05-19  
**状态:** 待实施

---

## 一、设计目标

将现有单页个人主页升级为具有顶部导航的个人IP网站，支持"博主介绍"与"项目介绍"两个模块切换，为后续扩展更多模块预留空间。

## 二、设计决策汇总

| 决策项 | 选择 | 说明 |
|--------|------|------|
| 导航栏风格 | 深色实底 + 青色 pill 选中 | 不透明深蓝底(#0a1a35)，底部青色分隔线，选中项用青色 pill 标签 |
| 项目卡片 | 暗色横排列表 | 与 Hero 暗色区呼应，横向布局：图标 + 信息 + 详情按钮 |
| 页面切换 | 淡入淡出 (opacity) | 旧内容淡出 → 新内容淡入，transition 0.3s ease |
| 博主介绍内容 | 保留所有现有 section | Hero、基础能力、前端框架、生态扩展、工程化、组件库、Footer 均不动 |
| Hero 区域 | 固定不变 | 切换 tab 时 Hero 始终可见，仅下方内容区域做淡入淡出 |
| 右侧导航 | 仅博主介绍页显示 | 切到项目介绍时自动隐藏 side-nav dots |

## 三、架构设计

### 3.1 页面结构

```
┌─────────────────────────────────────┐
│  Sticky Nav Bar (60px, z-index: 100)│  ← 始终可见
│  白·IT白袍   [博主介绍] [项目介绍]    │
├─────────────────────────────────────┤
│                                     │
│  Hero 区域 (始终可见)                │  ← 固定不动
│                                     │
├─────────────────────────────────────┤
│                                     │
│  ↓ 切换区域 (淡入淡出过渡) ↓          │  ← 以下内容切换
│                                     │
│  [博主介绍 tab]     [项目介绍 tab]    │
│   - 基础能力         - 项目卡片列表   │
│   - 前端框架                        │
│   - 生态扩展                        │
│   - 工程化                          │
│   - 组件库                          │
│   - Footer+地图                     │
│                                     │
└─────────────────────────────────────┘
```

### 3.2 导航栏组件

- **位置:** `position: sticky; top: 0; z-index: 100`
- **高度:** 60px
- **背景:** `#0a1a35` 实色，底部 `1px solid rgba(0,180,216,0.2)` 分隔
- **左侧:** 红色古风印章小图标(18px) + "IT白袍" 文字
- **右侧:** 导航项
  - "博主介绍" — 默认激活，渲染为青色 pill: `background: rgba(0,180,216,0.12); color: #00b4d8; border-radius: 6px; padding: 4px 12px`
  - "项目介绍" — 非激活态，灰色文字 `rgba(255,255,255,0.6)`
- **响应式:** 移动端缩小高度至 50px，字号和间距等比缩小

### 3.3 页面切换机制

- 两个内容容器: `#tab-blogger` / `#tab-projects`
- CSS 类控制显示: `.tab-content { opacity: 0; pointer-events: none; transition: opacity 0.3s ease }` / `.tab-content.active { opacity: 1; pointer-events: auto }`
- JS: 点击导航 → 切换两个容器的 `.active` 类
- Hero 区域位于两个容器之外，不参与切换
- 切换到"项目介绍"时，side-nav 添加 `.hidden` 类隐藏

### 3.4 项目卡片数据模型

```js
var projects = [
  {
    id: 'proj-1',
    name: '项目名称',
    desc: '简短描述说明',
    icon: '🚀',
    tags: ['Vue 3', 'TypeScript'],
    demoUrl: 'https://xxx.com'
  }
];
```

卡片渲染为 HTML 字符串插入容器。后续可改为从 JSON 文件或 API 加载。

### 3.5 项目卡片样式

- 单列列表布局（`.projects-list { display: flex; flex-direction: column; gap: 16px }`）
- 暗色底卡片: `background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.08); border-radius: 12px`
- 横向 flex 布局: 左侧 emoji/截图(64px 圆角方形) → 标题+描述+标签 → 右侧"详情"按钮
- 悬停效果: 边框高亮为青色，轻微上移
- "详情"按钮: 橙色文字 + 橙色描边 → hover 填充橙色渐变

## 四、实施要点

### 4.1 导航栏标记注入

在 `<body>` 开头（entrance overlay 之后）插入导航 HTML：

```html
<nav class="site-nav" id="siteNav">
  <div class="site-nav__inner container">
    <a class="site-nav__brand" href="#">
      <span class="site-nav__seal">白</span>
      <span>IT白袍</span>
    </a>
    <div class="site-nav__links">
      <button class="site-nav__link is-active" data-tab="blogger">博主介绍</button>
      <button class="site-nav__link" data-tab="projects">项目介绍</button>
    </div>
  </div>
</nav>
```

### 4.2 内容包裹

- Hero 区域保持原样 `<header class="hero">`
- Hero 之后的所有 section 用 `<div id="tab-blogger" class="tab-content active">` 包裹
- 新建 `<div id="tab-projects" class="tab-content">` 放置项目卡片容器

### 4.3 JavaScript 新增逻辑

- 导航 tab 点击事件: 切换 `.is-active` 类 + 对应 `.tab-content` 的 `.active` 类
- 切换时控制 side-nav 显隐
- 项目卡片渲染函数: 遍历 projects 数组生成 HTML

### 4.4 CSS 新增

- `.site-nav` 系列样式 — 导航栏全套
- `.tab-content` 切换动画
- `.project-card` 系列样式 — 项目卡片
- 响应式: 768px / 480px 断点适配
- 深色区域自动切换 side-nav 亮色模式已存在，无需改动

## 五、可扩展方向（后续迭代）

1. **更多导航 tab** — blog 文章页、开源项目页、联系页等，只需在导航添加项目 + 新增 tab-content 容器
2. **项目筛选/搜索** — 按技术栈标签筛选，关键词搜索
3. **项目详情弹窗** — 不跳转，直接在浮层中展示详情（截图、技术细节）
4. **数据驱动** — 项目数据从 JSON 或 API 加载，支持动态增删
5. **Blog 文章列表** — 额外 tab 展示 CSDN 或自建博客文章
6. **国际化** — 中/英切换
7. **夜间/日间模式** — 主题切换

## 六、文件影响范围

| 文件 | 操作 | 说明 |
|------|------|------|
| `index.html` | 修改 | 添加导航 HTML、CSS、JS，包裹 tab 容器，新增项目卡片渲染逻辑 |

当前项目仅有 `index.html` 单文件，所有改动集中在此文件。

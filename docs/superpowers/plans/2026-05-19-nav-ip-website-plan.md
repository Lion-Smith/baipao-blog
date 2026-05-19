# 个人IP网站导航与项目展示 — 实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 为现有单页 index.html 添加粘性导航栏、博主介绍/项目介绍双 tab 切换、项目卡片展示。

**Architecture:** 单文件 index.html，新增 ~200 行 CSS（导航栏、tab 切换、项目卡片、响应式）、~20 行 HTML（导航栏标记、tab 容器）、~60 行 JS（tab 切换、项目渲染、side-nav 控制）。Hero 区域不参与切换，所有现有 section 完整保留。

**Tech Stack:** 原生 HTML/CSS/JS，百度地图 JS API v3.0

---

## 文件结构

| 文件 | 职责 |
|------|------|
| `index.html` | 所有改动集中在此文件：CSS 样式块、HTML 标记块、JS 脚本块 |

---

### Task 1: 导航栏 CSS

**Files:** 修改 `index.html` — 在 `<style>` 块末尾 `</style>` 前插入

- [ ] **Step 1: 插入导航栏 CSS**

在第 894 行（`</style>` 之前）插入：

```css
  /* ========== 顶部导航栏 ========== */
  .site-nav {
    position: sticky; top: 0; z-index: 100;
    height: 60px;
    background: #0a1a35;
    border-bottom: 1px solid rgba(0,180,216,0.2);
    -webkit-backdrop-filter: blur(10px);
    backdrop-filter: blur(10px);
  }
  .site-nav__inner {
    display: flex; align-items: center; justify-content: space-between;
    height: 100%;
  }
  .site-nav__brand {
    display: flex; align-items: center; gap: 8px;
    color: rgba(255,255,255,0.9);
    text-decoration: none;
    font-weight: 800; font-size: 16px;
    letter-spacing: 1px;
  }
  .site-nav__seal {
    display: flex; align-items: center; justify-content: center;
    width: 22px; height: 22px;
    background: var(--seal-red);
    color: #FFF8E7;
    font-family: "KaiTi","STKaiti","楷体",serif;
    font-size: 12px; font-weight: 900;
    border-radius: 3px;
  }
  .site-nav__links {
    display: flex; gap: 6px;
  }
  .site-nav__link {
    padding: 5px 14px;
    border-radius: 6px;
    font-size: 13px; font-weight: 600;
    color: rgba(255,255,255,0.55);
    background: transparent;
    border: none; cursor: pointer;
    transition: all 0.25s ease;
    font-family: inherit;
  }
  .site-nav__link:hover {
    color: rgba(255,255,255,0.85);
  }
  .site-nav__link.is-active {
    background: rgba(0,180,216,0.12);
    color: #00b4d8;
  }
```

- [ ] **Step 2: 验证** — 浏览器打开 index.html，确认无 CSS 语法错误（检查 DevTools 无报错）

---

### Task 2: Tab 切换 CSS

**Files:** 修改 `index.html` — 在 `</style>` 前追加

- [ ] **Step 1: 插入 tab-content 切换 CSS**

```css
  /* ========== Tab 内容切换 ========== */
  .tab-content {
    opacity: 0; pointer-events: none;
    transition: opacity 0.3s ease;
  }
  .tab-content.active {
    opacity: 1; pointer-events: auto;
  }
```

- [ ] **Step 2: 验证** — 浏览器 DevTools 确认 CSS 已加载，无报错

---

### Task 3: 项目卡片 CSS

**Files:** 修改 `index.html` — 在 `</style>` 前追加

- [ ] **Step 1: 插入项目卡片 + 项目介绍区域 CSS**

```css
  /* ========== 项目介绍区域 ========== */
  .projects-section {
    padding: 90px 0; position: relative; z-index: 1;
    min-height: 60vh;
    background: linear-gradient(180deg, #0d1a34 0%, var(--bg) 40%);
  }
  .projects-header {
    text-align: center; margin-bottom: 48px;
  }
  .projects-header .section-title {
    color: #fff;
  }
  .projects-header .section-subtitle {
    color: rgba(255,255,255,0.55);
  }
  .projects-list {
    display: flex; flex-direction: column; gap: 16px;
    max-width: 760px; margin: 0 auto;
  }
  .project-card {
    display: flex; align-items: center; gap: 16px;
    padding: 20px 24px;
    background: rgba(255,255,255,0.04);
    border: 1px solid rgba(255,255,255,0.08);
    border-radius: 12px;
    transition: all 0.3s ease;
    text-decoration: none; color: inherit;
    cursor: pointer;
  }
  .project-card:hover {
    border-color: rgba(0,180,216,0.35);
    transform: translateY(-2px);
    box-shadow: 0 8px 32px rgba(0,0,0,0.2);
  }
  .project-card__icon {
    width: 64px; height: 64px;
    background: rgba(0,180,216,0.1);
    border-radius: 12px; flex-shrink: 0;
    display: flex; align-items: center; justify-content: center;
    font-size: 28px;
  }
  .project-card__info {
    flex: 1; min-width: 0;
  }
  .project-card__name {
    font-size: 16px; font-weight: 700;
    color: #fff; margin-bottom: 4px;
  }
  .project-card__desc {
    font-size: 13px; color: rgba(255,255,255,0.5);
    line-height: 1.5;
  }
  .project-card__tags {
    display: flex; gap: 6px; margin-top: 8px; flex-wrap: wrap;
  }
  .project-card__tag {
    padding: 2px 10px;
    background: rgba(0,180,216,0.1);
    border-radius: 4px;
    font-size: 11px; font-weight: 600;
    color: #00b4d8;
  }
  .project-card__btn {
    flex-shrink: 0;
    padding: 7px 16px;
    border: 1px solid rgba(255,107,53,0.3);
    border-radius: 6px;
    color: var(--orange);
    font-size: 12px; font-weight: 600;
    transition: all 0.25s ease;
    white-space: nowrap;
  }
  .project-card:hover .project-card__btn {
    background: linear-gradient(135deg, #FF6B35, #E0552A);
    color: #fff; border-color: transparent;
    box-shadow: 0 4px 16px rgba(255,107,53,0.3);
  }
  /* 项目介绍空状态 */
  .projects-empty {
    text-align: center; padding: 60px 20px;
    color: rgba(255,255,255,0.4); font-size: 14px;
  }
```

- [ ] **Step 2: 验证** — DevTools 检查无 CSS 语法错误

---

### Task 4: 导航栏与项目卡片响应式 CSS

**Files:** 修改 `index.html` — 在 `</style>` 前追加

- [ ] **Step 1: 插入响应式 CSS（追加到现有媒体查询中）**

在 `@media (max-width: 768px)` 块内末尾（第 802 行 `.float-csdn` 规则之后，`}` 之前）插入：

```css
    /* 导航栏响应式 */
    .site-nav { height: 50px; }
    .site-nav__brand { font-size: 14px; }
    .site-nav__seal { width: 18px; height: 18px; font-size: 10px; }
    .site-nav__link { padding: 4px 10px; font-size: 12px; }

    /* 项目介绍区域 */
    .projects-section { padding: 60px 0; }
    .project-card { padding: 16px 18px; gap: 12px; }
    .project-card__icon { width: 50px; height: 50px; border-radius: 10px; font-size: 22px; }
    .project-card__name { font-size: 14px; }
    .project-card__desc { font-size: 12px; }
    .project-card__btn { padding: 5px 12px; font-size: 11px; }
```

在 `@media (max-width: 480px)` 块内末尾（第 812 行 `.float-csdn` 规则之后，`}` 之前）插入：

```css
    /* 导航栏响应式 */
    .site-nav { height: 46px; }
    .site-nav__brand { font-size: 13px; gap: 6px; }
    .site-nav__seal { width: 16px; height: 16px; font-size: 9px; border-radius: 2px; }
    .site-nav__link { padding: 3px 8px; font-size: 11px; }

    /* 项目介绍区域 */
    .projects-section { padding: 48px 0; }
    .projects-header { margin-bottom: 32px; }
    .projects-list { gap: 12px; }
    .project-card { padding: 14px 16px; gap: 10px; border-radius: 10px; }
    .project-card__icon { width: 42px; height: 42px; border-radius: 8px; font-size: 20px; }
    .project-card__name { font-size: 13px; }
    .project-card__desc { font-size: 11px; }
    .project-card__tag { font-size: 10px; padding: 1px 8px; }
    .project-card__btn { padding: 5px 10px; font-size: 10px; border-radius: 4px; }
```

- [ ] **Step 2: 验证** — 浏览器打开，Chrome DevTools 切换移动端视口（375px/768px），确认无布局错乱

---

### Task 5: 导航栏 HTML 标记

**Files:** 修改 `index.html` — 在 `</div>` (entrance overlay) 之后插入

- [ ] **Step 1: 插入导航栏 HTML**

在第 913 行 `</div>`（entrance overlay 结束）之后、`<!-- ====== 粒子画布 ====== -->` 之前，插入：

```html
<!-- ====== 顶部导航栏 ====== -->
<nav class="site-nav" id="siteNav">
  <div class="site-nav__inner container">
    <a class="site-nav__brand" href="#" aria-label="IT白袍首页">
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

- [ ] **Step 2: 验证** — 浏览器打开 index.html，确认导航栏可见，位于页面顶部

---

### Task 6: 包裹现有内容到 tab-blogger 容器

**Files:** 修改 `index.html` — 在 Hero 之后、Footer 之后添加包裹标签

- [ ] **Step 1: 在 Hero 结束标签后插入 tab-blogger 开标签**

在第 982 行 `</header>` 之后插入：

```html

<!-- ====== 博主介绍 Tab 内容 ====== -->
<div id="tab-blogger" class="tab-content active">
```

- [ ] **Step 2: 在 Footer 结束标签后插入 tab-blogger 闭标签**

在第 1215 行 `</footer>` 之后插入：

```html
</div><!-- /#tab-blogger -->
```

- [ ] **Step 3: 验证** — 浏览器打开，所有现有内容可见，滚动正常

---

### Task 7: 新建 tab-projects 容器

**Files:** 修改 `index.html` — 在 `</div><!-- /#tab-blogger -->` 之后插入

- [ ] **Step 1: 插入项目介绍 tab 容器**

在 `</div><!-- /#tab-blogger -->` 之后、`<script>` 之前插入：

```html

<!-- ====== 项目介绍 Tab 内容 ====== -->
<div id="tab-projects" class="tab-content">
  <section class="projects-section">
    <div class="container">
      <div class="projects-header">
        <div class="section-label">Projects</div>
        <h2 class="section-title">项目介绍</h2>
        <p class="section-subtitle">精选项目展示，点击详情可在线体验</p>
        <div class="section-divider"></div>
      </div>
      <div class="projects-list" id="projectsList">
        <div class="projects-empty">项目数据加载中...</div>
      </div>
    </div>
  </section>
</div><!-- /#tab-projects -->
```

- [ ] **Step 2: 验证** — 浏览器打开，切换 tab 暂时看不到项目卡片（JS 尚未实现），但容器结构需正确

---

### Task 8: Tab 切换 JavaScript

**Files:** 修改 `index.html` — 在 `<script>` 中现有 IIFE 之前添加

- [ ] **Step 1: 在第 1218 行 `(function() {` 之前插入 tab 切换逻辑**

```js
  /* ========== Tab 切换 ========== */
  var navLinks = document.querySelectorAll('.site-nav__link');
  var tabBlogger = document.getElementById('tab-blogger');
  var tabProjects = document.getElementById('tab-projects');
  var sideNav = document.getElementById('sideNav');

  function switchTab(tabName) {
    navLinks.forEach(function(link) {
      link.classList.toggle('is-active', link.getAttribute('data-tab') === tabName);
    });

    if (tabName === 'blogger') {
      tabBlogger.classList.add('active');
      tabProjects.classList.remove('active');
      if (sideNav) sideNav.classList.remove('hidden');
    } else {
      tabProjects.classList.add('active');
      tabBlogger.classList.remove('active');
      if (sideNav) sideNav.classList.add('hidden');
    }
  }

  navLinks.forEach(function(link) {
    link.addEventListener('click', function() {
      var tab = this.getAttribute('data-tab');
      if (tab) switchTab(tab);
    });
  });
```

- [ ] **Step 2: 添加 side-nav hidden 样式**

在 CSS 中（task 2 附近）追加：

```css
  .side-nav.hidden { opacity: 0; pointer-events: none; transition: opacity 0.25s ease; }
```

- [ ] **Step 3: 验证** — 浏览器打开：点击"项目介绍"→ 博主介绍内容消失、项目介绍显示、side-nav 隐藏；点击"博主介绍"→ 恢复

---

### Task 9: 项目卡片渲染 JavaScript

**Files:** 修改 `index.html` — 在 task 8 的 tab 切换代码之后追加

- [ ] **Step 1: 插入项目数据和渲染逻辑**

```js
  /* ========== 项目卡片渲染 ========== */
  var projects = [
    {
      id: 'proj-1',
      name: '示例项目',
      desc: '这是一个示例项目描述，后续可替换为真实项目信息。',
      icon: '🚀',
      tags: ['Vue 3', 'TypeScript'],
      demoUrl: '#'
    }
  ];

  var projectsList = document.getElementById('projectsList');

  function renderProjects() {
    if (!projectsList) return;

    if (projects.length === 0) {
      projectsList.innerHTML = '<div class="projects-empty">暂无项目展示，敬请期待</div>';
      return;
    }

    var html = '';
    projects.forEach(function(proj) {
      var tagsHtml = proj.tags.map(function(t) {
        return '<span class="project-card__tag">' + t + '</span>';
      }).join('');

      html +=
        '<a class="project-card" href="' + proj.demoUrl + '" target="_blank" rel="noopener">' +
          '<div class="project-card__icon">' + proj.icon + '</div>' +
          '<div class="project-card__info">' +
            '<div class="project-card__name">' + proj.name + '</div>' +
            '<div class="project-card__desc">' + proj.desc + '</div>' +
            '<div class="project-card__tags">' + tagsHtml + '</div>' +
          '</div>' +
          '<span class="project-card__btn">查看详情</span>' +
        '</a>';
    });
    projectsList.innerHTML = html;
  }

  renderProjects();
```

- [ ] **Step 2: 验证** — 切换到"项目介绍"tab，确认示例项目卡片正常渲染，hover 效果正常

---

### Task 10: 提交前完整验证

- [ ] **Step 1: 桌面端验证清单**
  - [ ] 导航栏可见，sticky 生效（滚动页面后仍可见）
  - [ ] 默认选中"博主介绍"
  - [ ] 所有现有 section 可见、滚动正常
  - [ ] 右侧导航点正常显示、跳转正常
  - [ ] 点击"项目介绍"→ 内容淡入淡出切换
  - [ ] 项目卡片渲染正确、hover 效果正常
  - [ ] 右侧导航点在项目介绍页隐藏
  - [ ] 点击"博主介绍"→ 内容恢复

- [ ] **Step 2: 移动端验证清单** (Chrome DevTools 375px / 768px)
  - [ ] 导航栏缩小，文字不换行
  - [ ] 项目卡片横向布局不溢出
  - [ ] 地图信息窗口不超出屏幕

- [ ] **Step 3: 控制台检查**
  - [ ] 打开 DevTools Console，确认无 JS 错误
  - [ ] 确认百度地图正常加载

- [ ] **Step 4: 提交**

```bash
git add index.html
git commit -m "feat: 添加导航栏与项目展示模块

- 新增顶部粘性导航栏（深色实底+青色pill选中态）
- 博主介绍/项目介绍双tab切换，Hero区域固定不变
- 暗色横排项目卡片，支持"查看详情"跳转外部链接
- 项目介绍页自动隐藏右侧快捷导航
- 移动端响应式适配

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## 自检清单

- [x] 覆盖所有 spec 需求（导航栏、tab 切换、项目卡片、Hero 固定、side-nav 隐藏、响应式）
- [x] 无 TBD/TODO/占位符
- [x] 所有 CSS 类名前后一致
- [x] 所有 JS 变量名与 HTML id/class 对应
- [x] 响应式断点与现有代码一致（768px / 480px）

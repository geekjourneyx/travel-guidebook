# 排版规范——融合 Claude 设计语言

> 本文件包含路书 HTML 的完整 CSS 规范。Stage 4 写作时参照此文件设置所有样式。
> 核心原则：**排版即设计**——不依赖图片，通过字体层级、功能色、SVG 装饰和 CSS 伪元素创造设计感。

## 目录

- [外部资源引入](#外部资源引入仅-3-行)
- [页面尺寸与布局](#页面尺寸与布局)
- [字体系统](#字体系统)
- [字号与行高](#字号与行高)
- [配色方案](#配色方案claude-暖色调--路书功能色)
- [配色应用规则](#配色应用规则)
- [装饰元素样式](#装饰元素样式)
- [分页规则](#分页规则优化版)
- [打印适配](#打印适配)

---

## 零外部依赖（重要）

**不引入任何 CDN 资源。** 所有字体、图标、装饰均内联，确保 PDF 秒开。

- 字体：使用系统字体栈，不加载 Google Fonts
- 图标：使用内联 SVG 图标函数，不加载图标字体 CDN
- 装饰：所有 SVG 内联在 HTML 中
- HTML `<head>` 中不出现任何 `<link>` 标签

```html
<!-- head 中只有 meta + title + <style>，零外部链接 -->
```

---

## 页面尺寸与布局

```css
@page {
  size: A4;
  margin: 2cm 1.8cm 2.5cm 1.8cm;
}

body {
  max-width: 680px;
  margin: 0 auto;
  padding: 0 1.5rem;
}
```

---

## 字体系统

```css
:root {
  --font-serif: 'Songti SC', 'STSong', 'SimSun', 'Noto Serif CJK SC', serif;
  --font-sans: 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'Noto Sans CJK SC', sans-serif;
  --font-mono: 'SF Mono', 'Menlo', 'Consolas', 'Courier New', monospace;
}

/* 标题用衬线体，正文用无衬线，功能信息用等宽 */
.cover-title,
.day-title,
h1, h2, h3 {
  font-family: var(--font-serif);
}

body, p, li {
  font-family: var(--font-sans);
}

.meta-item,
.page-number,
.stop-badge {
  font-family: var(--font-mono);
}
```

---

## 字号与行高

```css
:root {
  --text-base: 17px;
  --text-sm: 15px;
  --text-xs: 14px;
  --text-lg: 24px;
  --text-xl: 28px;
  --text-2xl: 32px;
  --text-3xl: 48px;
  
  --leading-tight: 1.25;
  --leading-normal: 1.85;
  --leading-relaxed: 1.70;
}

body {
  font-size: var(--text-base);
  line-height: var(--leading-normal);
}

@media print {
  body {
    font-size: 11.5pt;
  }
}
```

---

## 配色方案——Claude 暖色调 + 路书功能色

```css
:root {
  /* 基础色——继承 Claude */
  --parchment: #f5f4ed;        /* 页面背景：温暖的羊皮纸 */
  --ivory: #faf9f5;            /* 卡片背景 */
  --near-black: #141413;       /* 主文字 */
  --terracotta: #c96442;       /* 品牌色/主 CTA/日程卡片强调 */
  --coral: #d97757;            /* 链接/强调 */
  
  /* 功能色——路书专属 */
  --route-blue: #4a7c8a;       /* 路线/交通信息——沉稳的青蓝 */
  --food-amber: #b8860b;       /* 美食推荐——温暖的琥珀色 */
  --stay-olive: #6b7c5e;       /* 住宿信息——安静的橄榄绿 */
  --alert-rust: #a0522d;       /* 警告/注意——深赭色 */
  --nature-sage: #8fbc8f;      /* 自然/户外——鼠尾草绿 */
  
  /* 中性色——保持 Claude 暖调，但对比度增强（户外可读性） */
  --text-primary: #2d2b28;
  --text-secondary: #524f4a;
  --text-tertiary: #87867f;
  --border-cream: #e8e6dc;
  --border-warm: #d1cfc5;
}
```

---

## 配色应用规则

```css
/* 页面背景 + 纸张纹理 */
body {
  background-color: var(--parchment);
  background-image:
    /* 极淡的斜纹 —— 模拟纸张纤维 */
    repeating-linear-gradient(
      120deg,
      transparent,
      transparent 2px,
      rgba(139, 119, 90, 0.015) 2px,
      rgba(139, 119, 90, 0.015) 3px
    ),
    /* 大面积的温暖渐变 —— 模拟纸张不均匀的色调 */
    radial-gradient(
      ellipse at 20% 50%,
      rgba(201, 100, 66, 0.03) 0%,
      transparent 70%
    ),
    radial-gradient(
      ellipse at 80% 20%,
      rgba(184, 134, 11, 0.02) 0%,
      transparent 60%
    );
}

/* 日程卡片 */
.day-card {
  background: var(--ivory);
  border: 1px solid var(--border-cream);
  box-shadow: 0px 0px 0px 1px var(--border-warm);
}

/* 功能色应用 */
.route-description { border-left: 3px solid var(--route-blue); padding-left: 1rem; }
.food-card { border-left: 3px solid var(--food-amber); padding-left: 1rem; }
.stay-card { border-left: 3px solid var(--stay-olive); padding-left: 1rem; }
.tips-box { 
  background: rgba(160, 82, 45, 0.08); 
  border-left: 3px solid var(--alert-rust);
  padding: 1rem 1.5rem;
}

/* 图标颜色 */
.ti { color: inherit; }
.meta-item .ti { color: var(--route-blue); }
.food-card .ti { color: var(--food-amber); }
.stay-card .ti { color: var(--stay-olive); }
.tips-box .ti { color: var(--alert-rust); }
```

---

## 装饰元素样式

```css
/* 指南针 */
.compass-rose {
  display: flex;
  justify-content: center;
  margin: 2rem 0;
  color: var(--text-tertiary);
}

/* 分隔线 */
.divider {
  display: flex;
  justify-content: center;
  margin: 2.5rem 0;
  color: var(--border-warm);
}
.divider-flourish { color: var(--terracotta); opacity: 0.5; }
.divider-diamond { color: var(--text-tertiary); }
.divider-dots { color: var(--border-warm); margin: 1.5rem 0; }

/* DAY 编号徽章 */
.day-badge {
  display: flex;
  justify-content: center;
  margin: 3rem 0 2rem;
}

/* 封面双线边框 */
.cover {
  position: relative;
  padding: 4rem 3rem;
  page-break-after: always;
}
.cover::before {
  content: "";
  position: absolute;
  top: 1.5rem; left: 1.5rem; right: 1.5rem; bottom: 1.5rem;
  border: 1.5px solid var(--terracotta);
  border-radius: 4px;
  opacity: 0.4;
}
.cover::after {
  content: "";
  position: absolute;
  top: 2rem; left: 2rem; right: 2rem; bottom: 2rem;
  border: 0.5px solid var(--terracotta);
  border-radius: 2px;
  opacity: 0.25;
}

/* 卡片角标装饰 */
.stop-card,
.food-card,
.stay-card {
  position: relative;
  background: var(--ivory);
  border-radius: 10px;
  padding: 1.5rem;
  margin-bottom: 2rem;
}
.stop-card::before,
.food-card::before,
.stay-card::before {
  content: "";
  position: absolute;
  top: 0; left: 0;
  width: 24px; height: 24px;
  border-top: 2px solid var(--terracotta);
  border-left: 2px solid var(--terracotta);
  border-top-left-radius: 10px;
  opacity: 0.4;
}
.stop-card::after,
.food-card::after,
.stay-card::after {
  content: "";
  position: absolute;
  bottom: 0; right: 0;
  width: 24px; height: 24px;
  border-bottom: 2px solid var(--terracotta);
  border-right: 2px solid var(--terracotta);
  border-bottom-right-radius: 10px;
  opacity: 0.4;
}

/* 首字下沉 */
.route-description > p:first-of-type::first-letter {
  float: left;
  font-family: var(--font-serif);
  font-size: 3.5em;
  font-weight: 900;
  line-height: 0.8;
  margin-right: 0.08em;
  margin-top: 0.05em;
  color: var(--terracotta);
}

/* 笔记区虚线 */
.journal-line {
  width: 100%;
  height: 1.8rem;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='600' height='2' viewBox='0 0 600 2'%3E%3Cline x1='0' y1='1' x2='600' y2='1' stroke='%23d1cfc5' stroke-width='0.5' stroke-dasharray='8 4'/%3E%3C/svg%3E");
  background-repeat: repeat-x;
  background-position: bottom;
  background-size: 600px 2px;
}

/* 徽章 */
.stop-badge {
  display: inline-flex;
  align-items: center;
  gap: 0.3em;
  background: var(--terracotta);
  color: var(--ivory);
  font-family: var(--font-mono);
  font-size: 0.75rem;
  font-weight: 500;
  letter-spacing: 0.1em;
  padding: 0.25em 0.75em;
  border-radius: 3px;
  text-transform: uppercase;
}

/* 功能信息标签 */
.meta-item {
  display: inline-flex;
  align-items: center;
  gap: 0.35em;
  font-family: var(--font-mono);
  font-size: 0.88rem;
  color: var(--text-secondary);
  padding: 0.2em 0.6em;
  background: rgba(232, 230, 220, 0.6);
  border-radius: 4px;
  margin-right: 0.5em;
  margin-bottom: 0.4em;
}
```

---

## 分页规则（优化版）

```css
/* 1. 封面独占一页 */
.cover {
  page-break-after: always;
  break-after: page;
}

/* 2. 旅程概览自成一页 */
.overview {
  page-break-before: always;
  break-before: page;
}

/* 3. 行前准备——智能分页 */
.preparation {
  page-break-before: auto;
  break-before: auto;
  page-break-inside: avoid;
  break-inside: avoid;
}

/* 4. 每个 Day 章节从新页开始 */
.day-chapter {
  page-break-before: always;
  break-before: page;
}

/* 5. 防止卡片被拆分 */
.day-card,
.stop-card,
.food-card,
.stay-card,
.tips-box,
.journal-space {
  page-break-inside: avoid;
  break-inside: avoid;
}

/* 6. 防止表格被拆分 */
table, thead, tbody, tr {
  page-break-inside: avoid;
  break-inside: avoid;
}

/* 7. 标题不与正文分离 */
h1, h2, h3, h4 {
  page-break-after: avoid;
  break-after: avoid;
  orphans: 3;
  widows: 3;
}

/* 8. 深度游章节从新页开始 */
.deep-dive {
  page-break-before: always;
  break-before: page;
}

/* 9. 附录从新页开始 */
.appendix {
  page-break-before: always;
  break-before: page;
}

/* 10. 封底独占一页 */
.back-cover {
  page-break-before: always;
  break-before: page;
}

/* 11. 全局孤行控制 */
p {
  orphans: 3;
  widows: 3;
}

/* 12. 行前准备的子区块不拆分 */
.prep-section {
  page-break-inside: avoid;
  break-inside: avoid;
}
```

---

## 打印适配

```css
@media print {
  body { 
    font-size: 11.5pt; 
    color: var(--text-primary);
  }
  
  .content { 
    max-width: 100%; 
    padding: 0; 
  }

  /* 保留背景色——路书的暖色调是核心体验 */
  .day-card, .stop-card, .food-card, .stay-card, .tips-box {
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }

  /* 全局孤行/寡行控制 */
  p, li, dd {
    orphans: 3;
    widows: 3;
  }

  /* 标题不与正文分离 */
  h2, h3 {
    break-after: avoid;
  }

  /* 卡片类元素不拆分 */
  .timeline-card, .tip-card, .deep-dive-card {
    break-inside: avoid;
  }

  /* 行前准备单独起页（内容量大时优化） */
  .preparation-section {
    break-before: page;
  }

  /* 封面页无上边距 */
  @page :first {
    margin-top: 0;
  }
  
  /* 页码——Playwright 使用 displayHeaderFooter 覆盖此规则 */
  @page {
    @bottom-center {
      content: "— " counter(page) " —";
      font-family: var(--font-mono);
      font-size: 9pt;
      color: var(--text-tertiary);
    }
  }
}
```

---

## 图标与装饰系统（速度 &#43; 美观平衡）

**禁止使用 `<i class="ti ti-xxx">` 和图标字体 CDN。禁止加载 Google Fonts。** 零外部依赖，PDF 秒开。

### 设计原则：装饰用 SVG，功能用 CSS

| 类型 | 方案 | 示例 |
|------|------|------|
| 装饰性元素（每页仅1-2个） | 保留内联 SVG | 指南针、DAY 徽章、分隔线 |
| 功能性图标（页面内大量重复） | CSS 彩色圆点 | 地点标记、贴士标记、列表项 |

这样做的理由：一个指南针 SVG 不会影响速度，但 40 个复杂路径图标会让每页渲染慢 5 秒。用 CSS 圆点替代功能性图标，渲染量从几百个 SVG 路径降到几十个圆形。

### 装饰性 SVG（保留）

封面指南针、DAY 编号徽章、分隔线等装饰性 SVG 沿用原有设计。每个页面仅出现 1-2 次，不影响性能。

```
指南针：封面使用，仅 1 个
DAY 徽章：每天 1 个，共 N 个
分隔线：flourish/diamond/dots 三种，按需使用
```

### 功能性图标：CSS 彩色圆点系统

用 CSS 圆点替代所有功能性图标（列表项、meta 标签、贴士标记等）：

```css
/* 圆点图标系统 */
.dot { display: inline-block; width: 7px; height: 7px; border-radius: 50%; vertical-align: middle; margin-right: 6px; flex-shrink: 0; }
.dot-r { background: var(--terracotta); }  /* 品牌色圆点：亮点、必停 */
.dot-b { background: var(--route-blue); }  /* 路线色圆点：交通、位置 */
.dot-a { background: var(--food-amber); }  /* 美食色圆点 */
.dot-g { background: var(--stay-olive); }  /* 住宿色圆点 */
.dot-w { background: var(--alert-rust); }  /* 警告色圆点：贴士 */
```

HTML 用法：
```html
<!-- 列表项 -->
<li><span class="dot dot-r"></span>亮点文字</li>
<li><span class="dot dot-w"></span>贴士内容</li>

<!-- 信息标签 -->
<span class="info-tag"><span class="dot dot-b"></span>建议 2h</span>
<span class="info-tag"><span class="dot dot-a"></span>人均 ¥80</span>
```

### 徽章标签

必停/可选等标签用纯 CSS 实现：
```css
.badge { display: inline-block; background: var(--terracotta); color: var(--ivory);
  font-family: var(--font-mono); font-size: 12px; padding: 2px 10px;
  border-radius: 3px; letter-spacing: 1px; margin-bottom: 8px; }
.badge-alt { background: var(--route-blue); }
```

### DAY 编号（简化版）

保留装饰感但减少 SVG 复杂度：
```css
.day-label { text-align: center; margin: 1rem 0 1.5rem; }
.day-label span { display: inline-block; border: 2px solid var(--terracotta);
  border-radius: 12px; padding: 6px 24px; }
```
```html
<div class="day-label"><span>DAY <b style="font-size:28px;color:var(--terracotta)">01</b></span></div>
```

### 分隔线（保留 SVG）

三种分隔线（flourish/diamond/dots）保留 SVG 内联，每个页面仅 1-2 个，不影响性能。

## 背景处理

**禁止使用 CSS 渐变纹理模拟纸张。** 使用纯色背景：

```css
body { background-color: var(--parchment); /* 不加 background-image 渐变 */ }
```

渐变纹理会让 PDF 阅读器在每页重新渲染背景区域，是 PDF 变慢的重要原因之一。

## 分页规则（防截断）

在原有分页规则基础上，**强制所有块级元素不可拆分**：

```css
/* 核心规则：所有内容块不可被页面截断 */
.stop-card, .food-card, .stay-card, .tips-box, .journal-space,
.route-description, .timeline-box, .prep-section, .deep-dive-section,
.appendix-section, .overview-highlights { page-break-inside: avoid; break-inside: avoid; }

/* 标题紧跟内容 */
h2, h3, h4 { page-break-after: avoid; break-after: avoid; }

/* 每天、深度游、附录从新页开始 */
.day-chapter, .deep-dive, .appendix { page-break-before: always; break-before: page; }
```

> **写作要求**：
> - HTML 中禁止出现 `<link rel="stylesheet" href="...">` 和 `<i class="ti ti-`
> - 禁止在 body 上使用 background-image 渐变
> - 所有内容块必须带 `page-break-inside: avoid`
> - 功能性图标使用 CSS 圆点，仅装饰元素使用内联 SVG

# GitHub Pages 轻量导航页（双语）+ Substack/公众号承接 Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 用 GitHub Pages 上线一个双语轻量导航页，作为统一入口完成“自测筛查 → 分流到 Substack（英）/公众号（中）→ 获取免费资源包 → 促成订阅/关注”。

**Architecture:** 静态站点（纯 HTML/CSS/少量 JS）。入口页提供 30 秒自测与中英分流；英文导向 Substack 订阅与免费资源包文章；中文展示公众号二维码与关键词领取，并导向公众号文章（可选）。

**Tech Stack:** GitHub Pages（GitHub Actions 部署）、HTML/CSS、可选 vanilla JS、静态资源（PNG/SVG）。

---

## Preflight（开始前需要你提供/确认）

### Inputs（必须）
- 网站流量统计代码（如 GA4 Measurement ID 或 Plausible 脚本代码）
- 站点社交分享图（OG Image，1200x630，JPEG/PNG）：`og-image.jpg`
- 自定义域名（强烈建议作为必须项：如 `aphantasia.yourdomain.com`，提升信任度）
- Substack publication URL（例如：`https://yourname.substack.com/`）
- 中文公众号名称 + 关键词（例如：关键词：`资源包`）
- 公众号二维码图片（PNG，建议 800px+）：`wechat-oa-qr.png`

### Optional（可选）
- 自定义域名（例如：`aphantasia.example.com`）
- 站点名称与一句话定位（中英各一版）

---

## Task 1: 初始化静态站点目录结构

**Files:**
- Create: `site/index.html`
- Create: `site/en/index.html`
- Create: `site/assets/styles.css`
- Create: `site/assets/wechat-oa-qr.png` (placeholder，需要你替换为真实二维码)
- Create: `site/assets/favicon.svg` (optional)

**Step 1: 创建目录与空文件**

Run:
```bash
mkdir -p site/assets site/en
touch site/index.html site/en/index.html site/assets/styles.css
```
Expected: 文件存在，无报错。

**Step 2: 写核心 HTML 与 SEO 骨架（中文入口）**
- `<head>`：添加 Title, Meta description, Open Graph 标签，以及指向 `/en/` 的 `hreflang` 标签

Write `site/index.html`（核心模块）：
- 顶部：站点名 + 一句话定位（中文）
- 30 秒自测（3 题单选/滑块都可；先用单选最省事）
- 结果不做复杂计算：用“如果你经常选‘几乎没有画面’”的轻量提示（MVP）
- 主 CTA：展示二维码 + 关键词说明（关注公众号后回复关键词领取资源包）
- 次 CTA：进入英文入口（链接到 `/en/`）
- 页脚：免责声明（非医疗建议）+ 隐私说明（静态页不收集个人数据）

**Step 3: 写核心 HTML 与 SEO 骨架（英文入口）**
- `<head>`：对应英文 SEO 标签与 `hreflang` 标签

Write `site/en/index.html`：
- 英文版定位与 30 秒自测
- 主 CTA：按钮链接到 Substack（订阅）
- 次 CTA：回到中文入口（链接到 `/`）
- 可选：链接到“Free Starter Kit”那篇 Substack 免费文章

**Step 4: 写基础样式**

Write `site/assets/styles.css`：
- 版心（max-width）、字号层级、按钮样式
- 手机端优先（responsive）
- 让二维码区块清晰可扫（白底、足够留白）

**Step 5: 本地预览**

Run:
```bash
python3 -m http.server 5173
```
Open: `http://localhost:5173/site/`  
Expected: 页面可打开、布局正常、分流链接可用。

**Step 6: Commit**

```bash
git add site
git commit -m "feat: add GitHub Pages bilingual navigation MVP"
```

---

## Task 2: 增加统计埋点与数据透传（Must）
- 在 JS 中增加事件上报：自测完成事件（`Test_Completed`）、移动端复制微信按钮点击（`Wechat_Copy_Clicked`）、跳转点击（`Outbound_Click`）。

**Files:**
- Modify: `site/en/index.html`
- Modify: `site/index.html`
- Create: `site/assets/utm.js` (optional)

**Step 1: 约定 UTM 透传规则**
- 如果入口页 URL 带 `utm_source/utm_medium/utm_campaign`，则把这些 query 参数拼到 Substack 跳转链接后面。

**Step 2: 实现最小 JS（可选，但建议）**

Write `site/assets/utm.js`：
- 读取当前 `location.search`
- 白名单参数：`utm_source,utm_medium,utm_campaign,utm_content,utm_term`
- 生成 query string，追加到带 `data-utm-link` 的链接上

**Step 3: 在中英页引用脚本并标注链接**
- 在 Substack 链接上加 `data-utm-link`

**Step 4: 手动验证**
- 访问 `.../en/?utm_source=x&utm_campaign=y`，点击订阅按钮
Expected: 跳转 URL 含相同 UTM 参数。

**Step 5: Commit**
```bash
git add site
git commit -m "feat: propagate UTM params to outbound links"
```

---

## Task 3: 配置 GitHub Pages（GitHub Actions 部署）

**Files:**
- Create: `.github/workflows/pages.yml`

**Step 1: 添加 GitHub Pages Actions workflow**

Write `.github/workflows/pages.yml`（使用官方 Pages actions）：
- 触发：push 到 `main`
- build：把 `site/` 作为 artifact 上传
- deploy：部署到 GitHub Pages

**Step 2: 在 GitHub 仓库设置启用 Pages**
- Repository → Settings → Pages → Source 选择 “GitHub Actions”

**Step 3: 推送并观察 Actions**
Expected: workflow 通过，生成 Pages URL（例如 `https://<owner>.github.io/<repo>/`）。

**Step 4: Commit**
```bash
git add .github/workflows/pages.yml
git commit -m "chore: deploy site via GitHub Pages actions"
```

---

## Task 4: 内容承接（平台侧，不在代码里实现）

**Substack（英文）**
- Step 1: 创建 1 篇免费“Starter Kit”文章（可作为 Lead Magnet 落地页）
- Step 2: 设置 Welcome email：新订阅者自动收到 Starter Kit 链接
- Step 3: 在导航页英文入口加“Get the free starter kit”链接

**公众号（中文）**
- Step 1: 配置关键词自动回复：关键词（如 `资源包`）→ 返回 Starter Kit 内容/链接
- Step 2: 首篇文章：什么是 Aphantasia（入门支柱文），文末引导回复关键词领资源包
- Step 3: 导航页放二维码 + 关键词说明

---

## Task 5: 快速验收清单（Definition of Done）

- `site/index.html` 中文入口可用：二维码清晰、关键词说明明确、可跳英文
- `site/en/index.html` 英文入口可用：可跳转 Substack、可跳中文
- GitHub Pages 已部署并可访问
- Substack welcome email 已配置并能把 Starter Kit 交付给新订阅者
- 公众号关键词自动回复可交付 Starter Kit

---

## Execution Handoff

Plan complete and saved to `docs/plans/2026-02-28-github-pages-mvp-navigation-implementation-plan.md`.

Two execution options:
1) Subagent-Driven (this session) — use superpowers:subagent-driven-development per task, review between tasks  
2) Parallel Session (separate) — open new session and use superpowers:executing-plans with checkpoints

Which approach?


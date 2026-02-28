# Aphantasia 创作者从 0 到 1 执行 SOP（Checklist）

这份 SOP 将《产品设计计划》与《技术实施计划》转化为线性的执行清单。作为创作者/Indie Hacker，你需要从上到下一步步打勾（✅）完成。

## 阶段 0：物料与基建准备（Week 0）

此阶段不涉及写代码或文章，仅注册账号、准备域名和设计基础物料。

- [ ] **域名准备**
  - [ ] 构思并购买一个专业域名（如 `aphantasia.xxx.com` 或 `yourbrand.com`）
  - [ ] 将域名的 DNS 解析权转移给 Cloudflare（推荐，为了后续免费 HTTPS 与数据统计）
- [ ] **账号与平台注册**
  - [ ] 注册 GitHub 账号，并创建一个空的 Public 仓库（如 `aphantasia-site`）
  - [ ] 注册 Substack 账号，完成基础设置（Pub Name, Short Description）
  - [ ] 注册 微信公众号（订阅号），完成实名认证
  - [ ] 注册 流量统计平台账号（推荐免费的 Cloudflare Web Analytics，或 Plausible/GA4）并获取 Tracking ID/Script
- [ ] **设计基础物料（UI/图片）**
  - [ ] 设计一张网站 SEO 社交分享封面（Open Graph Image，尺寸：`1200x630`，PNG/JPG格式）
  - [ ] 从公众号后台下载并裁剪出清晰的 公众号二维码图片（尺寸：建议 `800x800`，白底无杂质）
  - [ ] （可选）设计一个极简的网站图标（Favicon.svg）

---

## 阶段 1：跑通内容承接“诱饵”（Week 1）

在网站上线前，必须先确保护城河（免费资源包）在下游已经配置好，否则引流进来的用户会面临“死胡同”。

- [ ] **撰写并定稿免费资源包（Lead Magnet）**
  - [ ] 撰写中文版 Starter Kit（包含：自测题误区澄清、10条学习/工作策略、给伴侣的解释话术模板）
  - [ ] 翻译并调整出英文版 Starter Kit（Free PDF 或直接作为一篇设为 Free 的 Substack 长文）
- [ ] **配置 Substack 承接自动化（英文）**
  - [ ] 在 Substack 中发布那篇 Starter Kit 文章，并设为所有人都可见（或者上传 PDF 到 Google Drive 开启共享）
  - [ ] 在 Substack 设置中找到 "Welcome Email"，编辑新用户订阅成功后收到的第一封邮件
  - [ ] 在 Welcome Email 中放入 Starter Kit 的获取链接，并做简单感谢
- [ ] **配置微信公众号承接自动化（中文）**
  - [ ] 在微信公众号后台，利用图文/第三方工具生成 Starter Kit 的图文消息或网盘链接
  - [ ] 在公众号后台设置“自动回复 - 关键词回复”
  - [ ] 规则设定：用户回复“资源包”或“测试” -> 自动推送 Starter Kit 链接/图文

---

## 阶段 2：开发与部署入口漏斗（Week 1-2）

按照 Implementation Plan 的架构，使用 AI 辅助开发极简静态单页。

- [ ] **编写代码（让 AI 执行）**
  - [ ] 在本地创建 `site` 目录结构 (`index.html`, `en/index.html`, `assets/`)
  - [ ] 完成中英双语页面骨架与 CSS 样式（优先适配手机端，保证响应式）
  - [ ] 补齐 SEO `Meta` 标签、`hreflang` 双语互链、`Open Graph` 社交分享配置
  - [ ] 引入 JS 实现 30 秒自测的“动态打分展示”功能（Aha Moment）
  - [ ] 英文页：嵌入 Substack iframe 订阅表单（无缝订阅）
  - [ ] 中文页：加入 User-Agent 判定。PC端显示二维码；移动端隐藏二维码，显示“一键复制公众号名称并去微信”的交互
  - [ ] 在 `analytics.js` 中接入你的统计代码（GA4/Plausible），并在“复制微信”、“Substack 订阅点击”上打点
- [ ] **部署到 GitHub Pages**
  - [ ] 配置 `.github/workflows/pages.yml`，设定 push 到 `main` 时自动打包
  - [ ] 将本地代码 Commit 并 Push 到 GitHub
  - [ ] 在 GitHub Repo Setting -> Pages 中开启 Actions 部署
  - [ ] 在 Settings -> Pages 中绑定你在【阶段 0】购买的 自定义域名
- [ ] **QA 与完整链路测试**
  - [ ] 手机浏览器打开中文版 -> 测算得分 -> 点击复制 -> 切到微信搜索 -> 回复关键词 -> 成功收到中文资源包
  - [ ] PC 浏览器打开英文版 -> 测算得分 -> 输入真实测试邮箱 -> 登录该邮箱 -> 成功收到 Substack 的 Welcome Email 与英文资源包
  - [ ] 登录统计后台，确认 `Test_Completed` 等事件上报成功

---

## 阶段 3：内容起飞与流量分发（Week 3-6）

基础设施跑通后，进入纯粹的内容生产与分发期。

- [ ] **完成支柱内容（Pillar Content）库**
  - [ ] 撰写 6 篇支柱长文（中英双语，如“什么是Aphantasia”、“学习策略”、“工作策略”）
  - [ ] 将 1 篇长文拆解为 2-3 篇短文/短视频脚本（案例故事、误区澄清）
  - [ ] 每周至少推送 1 封 Newsletter / 1 篇公众号推文
- [ ] **SEO 与被动收录**
  - [ ] 将你的自定义域名提交给 Google Search Console
  - [ ] 在小红书、X（Twitter）、Reddit 相关版块（如 r/Aphantasia）通过“科普/提问”的形式分发内容
  - [ ] **所有的 Call To Action 统一指向你的独立站域名**，让独立站完成自测与分发
- [ ] **数据监控（每周 1 次复盘）**
  - [ ] 查看入口页 PV / UV
  - [ ] 查看 转化率 = (Substack 新增订阅 + 微信新增关注) / 入口页 PV
  - [ ] 如果转化率 < 5%，调整 Landing Page 自测题目或优化 Copywriting

---

## 阶段 4：打通会员变现（Week 7-10）

当免费列表累积到一定阈值（比如 500-1000 免费订阅）时，开启内测转化。

- [ ] **社群与付费基建**
  - [ ] 建立中文深度会员微信群（或知识星球）
  - [ ] 建立英文 Discord Server，设置好 `introductions`, `questions` 等频道
  - [ ] 在 Substack 开启 Paid Subscription，设置月付 $5 / 年付 $50，并勾选接入 Discord
- [ ] **首次 Launch（内测 50 席位）**
  - [ ] 向当前的免费邮件列表/公众号列表发送“会员招募”预告
  - [ ] 策划并举办第 1 次 月度线上 AMA（Ask Me Anything）作为会员卖点
  - [ ] 跑通“付费 -> 入群 -> 持续获取专属资料”的闭环，收集首批内测用户的反馈，优化留存。
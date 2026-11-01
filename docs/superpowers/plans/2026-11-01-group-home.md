# Airdb Group Homepage Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Preserve the user's native continuous implementation preference, followed by one independent final review.

**Goal:** 按已认可视觉稿落地仅 airdb.group 首页，明确四组业务导航及合作入口。

**Architecture:** Astro 静态首页，首页内容与样式隔离。BaseLayout 以默认关闭的首页变体承载对应语言导航/页脚，不改变现有内页渲染默认值。

**Tech Stack:** Astro 7.3.5、TypeScript、原生 CSS、现有本机 Playwright；不添加依赖。

**Spec:** ../specs/2026-11-01-group-home.md

## Global Constraints

- 只重做 group 首页；内页正文、路由和现有联系功能不改。
- 域名表是用户认可的目标定位，不代表已上线能力、牌照、规模或法律隶属。
- 本轮增加中英文首页与真实语言切换，不增加外部追踪、表单后台、依赖或网站迁移。不虚构规模、客户、牌照、荣誉。
- 实现采用原生连续实施，最终独立评审；通过检查后按用户最新授权直接发布 main。

## Review Focus

1. 320px 窄屏与中文字体回退：两行标题、首屏 CTA 可见，不溢出（任务 2）。
2. 共享布局首页开关：六个内页保持既有语言、正文、导航和样式（任务 2）。
3. 无 JS / 键盘导航：链接和菜单仍可用、焦点可见、Escape 正确（任务 2）。
4. 示意图失效或加载缓慢：文字和合作入口仍可用；不冒充真实资产（任务 1、2）。
5. 目标域名未开通或定位不一致：不虚构能力、不生成死入口承诺，状态如实记录（任务 1、2）。

---

### Task 1: 首页内容与示意资产

**Files:**
- Create: `src/config/home.ts`（仅首页域名分组/方法/合作入口数据）。
- Create: `public/images/group-home/hero.webp`、`commerce.webp`、`agriculture.webp`、`fintech.webp`、`infrastructure.webp`（独立示意资产；不嵌入整张概念稿）。
- Create: `docs/superpowers/specs/2026-11-01-group-home-assets.md`（来源、生成记录、用途）。

**Interfaces:**
- Consumes: 已读取视觉稿与设计记录中的精确域名、标题、CTA 目标。
- Produces: `homeBusinesses` 四项数组（title、description、image、links: {label, href}[]），`homeMethods` 三项数组，`homeCooperation` 四项数组；静态图片路径供首页使用。

- [x] 核对当前 clean main、设计分支及本计划确认记录；不带入 broad 分支。
- [x] 只读检查 7 个域名 HTTP/重定向目标，记录无法访问或语义不一致之处；数据仍按用户目标标注，不自行改站。
- [x] 使用 imagegen 技能从认可稿制作农业主图与四张业务示意图；无文字徽章/假客户。生成后逐张检查像素和权限来源，输出 web 适用资产并注明示意。若工具不可用先报告，不拿未知图片顶替。
- [x] 写首页专用数据；保留所有 7 个目标域名、三步方法和四类现有咨询路径，避免将目标当作已运营能力。
- [x] 检查图像尺寸/体积、可读性、图像不可用时的文字方案；记录来源，提交本地内容与资产。

### Task 2: 页面与响应式实现、完整验收

**Files:**
- Modify: `src/pages/index.astro`（首屏、四组业务、方法、合作）。
- Modify: `src/layouts/BaseLayout.astro`（新增 `homeLocale?: "zh" | "en"`，默认 undefined；首页使用 zh-CN 与专用导航页脚）。
- Create: `src/styles/home.css`（仅 `.group-home` 下规则，必要 body/header/footer 首页作用域；不改全局变量默认值）。
- Test evidence: `../reports/group-home-browser.mjs`、截图与结果 JSON（不新增产品依赖）。

**Interfaces:**
- Consumes: 任务 1 的数据和图片，现有 `company.email`、BaseLayout 默认内页能力。
- Produces: 可访问的中文 `/` 首页；`#business`、`#method`、`#cooperation`；所有既有内页路径不变。

- [x] 写并运行浏览器验收基线：四视口，单一 h1、两种语言首屏两个 CTA 可见、7 个域名链接、四组结构、无溢出；保存旧首页及全部内页快照，确认基线确实捕获当前首屏/目标结构问题。
- [x] 按设计记录改首页 HTML 与 CSS；首页组件不再引入 Hero/ComplianceBand/ServiceMatrix/InfoBlocks，但不删除它们或改其共享数据。
- [x] 增加 BaseLayout 首页变体，默认 undefined；中文首页导航含现有公司与联系入口，移动菜单继续使用已有可访问行为，页脚保留必要内页链接；提供真实 / 与 /en/ 切换，验证刷新、返回和语言 metadata。
- [x] 渲染 320×640、390×844、768×1024、1440×900：标题两行、简介与两 CTA 首屏可见，业务网格单/双列恰当，无横向滚动；图片可延后加载，不能挡正文或按钮。
- [x] 测试 Tab/Enter/Escape、跳过导航、移动菜单、直接锚点、正常/减少动效；关闭 JS、阻断图片和外部字体后仍可读可操作。
- [x] 执行 `node node_modules/astro/bin/astro.mjs build` 和 `node node_modules/astro/bin/astro.mjs check`；全部页面构建成功、无新增类型错误/警告。仓库无 test 脚本，不虚称运行不存在的测试套件。
- [x] 运行浏览器检查与站内 href/锚点扫描；对 /about、/contact、/services、/solutions、/engineering、/404 做手机/桌面回归及正文比较，确认共享布局不泄漏首页样式。
- [x] 对照真实参考图人工查看四视口像素；图像边界明确、业务导航不暗示子公司或牌照、公益初心未变成核心定位。
- [x] 运行 `git diff --check`，安排一次独立最终评审，处理必须修正项并重跑受影响检查。
- [ ] 本地提交、记录 SHA、截图和真实可用预览；报告未测限制。安全推送分支并快进 main，跟踪准确 SHA 的 CI / Pages 并核对线上两种语言。

## 本轮计划自检

设计记录各段均映射任务 1/2；所有 7 个域名、合作入口、首屏约束、共享布局隔离、资产示意及内页回归都有对应步骤。用户随后已批准实施及直接发布，并要求完整中英文首页。

## 已批准的实施细化
新增 src/components/GroupHome.astro 共享两语言结构，src/pages/en/index.astro 英文入口。homeCopy 为按 locale 索引的完整文案对象。图片使用 imagegen 原生生成，cwebp 仅进行格式压缩，无其他图像编辑。独立评审后发布，无需再次确认。

## Validation record
- 8 static pages built; Astro check 18 files, 0 errors/warnings/hints.
- 30 bilingual viewport/navigation/no-JS checks passed; 153 internal links/anchors passed.
- Six preserved inner pages at 390/1440px: identical main text, identical image dimensions, decoded pixel channel delta <=3 (browser rasterization variation), no layout differences.
- Independent review findings (no-JS sticky overlap and untranslated eyebrows) corrected and rechecked; no-JS assertion compares heading top with visible header bottom.
- airdb.com verified in Chromium HTTP200 (generic request had403); airdb.cn has ERR_CERT_DATE_INVALID, explicitly noted in both languages. Five other business domains HTTP200.
- Image source prompts recorded in asset document; five WebP assets total approximately854KB.
- No physical iPhone/Safari validation. Existing English inner pages intentionally not translated.

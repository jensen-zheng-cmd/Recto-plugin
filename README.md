# Recto

Convert Zotero PDF papers into Markdown, generate AI summaries and Chinese translations, and build a searchable local paper library with bilingual reading — inside Obsidian.

把 Zotero 里的 PDF 论文转成 Markdown，生成 AI 摘要与中文译文，并在 Obsidian 里建立可检索的本地论文库与双语对照阅读。

## Features / 功能

- **Zotero import** — detect local Zotero storage, import bibliographic metadata, and keep a paper library index in your vault  
  **Zotero 导入** — 检测本机 Zotero storage，导入书目元数据，并在库内维护论文索引
- **PDF → Markdown** — convert selected papers via the Recto cloud service (desktop only)  
  **PDF → Markdown** — 通过 Recto 云端服务转换所选论文（仅桌面端）
- **AI summary & translation** — structured summaries plus Chinese translation with alignment-aware bilingual reading  
  **AI 摘要与翻译** — 结构化摘要、中文译文，以及对齐感知的双语对照阅读
- **Hub** — browse conversion status, open papers, and start convert/translate from one place  
  **Hub** — 在一处查看转换状态、打开论文、发起转换/翻译

## Requirements / 使用要求

- **Obsidian desktop** (`isDesktopOnly: true`). Node / local-file APIs are required.  
  **Obsidian 桌面端**（`isDesktopOnly: true`），需要 Node / 本地文件能力。
- **A Recto account** is required for conversion, summary, translation, and paid quotas. Browsing a local library you already built does not require being signed in.  
  **完整转换 / 摘要 / 翻译 / 付费额度需要 Recto 账号**。浏览你已落在本地的论文库不强制登录。
- **Network access** to Recto services (`rectoai.uk` / `api.rectoai.uk`).  
  **需要联网**访问 Recto 服务（`rectoai.uk` / `api.rectoai.uk`）。
- Optional: a local **Zotero** install with PDF files downloaded into Zotero storage (cloud-only attachments that were never downloaded locally cannot be converted).  
  可选：本机已安装 **Zotero**，且 PDF 已下载到 storage（未落盘的纯云端附件无法转换）。

## Account, free tier, and payments / 账号、免费额度与付费

- Sign-up and sign-in happen in the browser on the Recto account site; the plugin never asks you to type a password into Obsidian.  
  注册与登录在浏览器账号页完成；插件不会在 Obsidian 内收集密码。
- New accounts need **email verification** before the first Obsidian session.  
  新账号须**先验邮箱**才能进入 Obsidian 会话。
- **Basic** includes a limited free allowance so you can try conversion and reading. **Pro** / **Max** are optional paid plans (period membership; no auto-renew / no silent charge).  
  **Basic** 提供有限免费额度便于试用；**Pro** / **Max** 为可选付费档（周期会员，到期不自动续费、不静默扣款）。
- Plan prices and remaining allowance are loaded from the Recto backend (catalog can refresh without login; your membership and balance require login).  
  套餐价目与剩余额度由 Recto 后端提供（价目表可不登录刷新；会员与余额需登录）。
- In the Obsidian Community directory this plugin is labeled **Optional payments** — there is a free tier, but paid cloud processing is part of the product.  
  社区目录定价标签为 **Optional payments**：有免费档，但付费云端处理是产品的一部分。

## Network, privacy, and data retention / 联网、隐私与数据保留

Honest disclosure for Community review and for users:

### What leaves your computer / 什么会离开本机

- When you start a **conversion**, selected **PDF files** are uploaded to the **Recto backend**. To finish parsing, summary, and translation, that content **may be forwarded to third-party processing services** chosen by Recto (vendor names intentionally omitted here; they can change as the service evolves).  
  你发起**转换**时，所选 **PDF** 会上传到 **Recto 后端**；为完成解析、摘要与翻译，内容**可能转发至 Recto 选用的第三方处理服务**（此处不列厂商名；服务选型可能随运营调整）。
- When you start **translation-only**, the plugin uploads the paper’s **Sidecar** (structured text derived from the earlier parse), not the PDF again. That payload may likewise be processed by third-party services via the Recto backend.  
  **只翻译**时上传的是该篇的 **Sidecar**（此前解析得到的结构化文本），不再传 PDF；同样可能经 Recto 后端交由第三方处理。
- Account, billing, and session traffic also goes to Recto (`api.rectoai.uk` / `rectoai.uk`), including email for auth and payment handoff pages.  
  账号、计费与会话流量也走 Recto（含认证邮件与支付交接页）。

### What stays local / 什么留在本地

- Your Obsidian vault notes, generated Markdown / images / Sidecar, `papers.jsonl`, and plugin settings (`data.json` on disk) stay on your machine.  
  Obsidian 库内笔记、生成的 Markdown / 图片 / Sidecar、`papers.jsonl` 与插件设置（磁盘上的 `data.json`）留在本机。
- **Zotero**: Recto may read your local Zotero database and **storage folder outside the vault** (read-only import). It does not upload your whole Zotero library; only PDFs (or Sidecars) you explicitly queue for cloud processing are uploaded.  
  **Zotero**：Recto 可能读取 vault **之外**的本地 Zotero 数据库与 **storage**（只读导入）。不会整库上传；只有你明确加入云端处理队列的 PDF（或 Sidecar）才会上传。
- Recto does **not** ship client-side telemetry, ads, or a self-update channel separate from Obsidian’s normal Community Plugin updates.  
  插件**不做**客户端遥测、动态广告，也不在 Obsidian 社区更新机制之外另做自我更新。

### Retention / 保留策略

- **Task files** (uploaded PDFs, intermediates, result packages) are temporary. After the plugin acknowledges a successful write-back they are deleted promptly; if not pulled, results expire on a **24-hour** TTL. Failed-task files are kept briefly for retry, then purged (also on a **24-hour** window). Cancel deletes server-side task files immediately.  
  **任务文件**（上传的 PDF、中间产物、结果包）为临时数据：插件确认写回成功后尽快删除；未拉取的结果约 **24 小时**过期。失败任务文件短暂保留供重试后清理（同样约 **24 小时**）。取消任务会立即删除服务端任务文件。
- **Account records** (email, membership, credit ledger, orders) are kept so billing and support remain consistent. There is currently **no self-service “delete my account” button** in the product; contact the author if you need account closure.  
  **账号记录**（邮箱、会员、额度账本、订单）会保留以便计费与支持。产品目前**没有自助销户入口**；如需关闭账号请联系作者。
- Payment processing uses third-party payment rails through Recto’s checkout pages; Recto does not ask the plugin to store card numbers.  
  支付经 Recto 结账页走第三方支付通道；插件不采集或存储银行卡号。

## Install / 安装

### From Obsidian Community Plugins (after listing)

1. Settings → Community plugins → Browse → search **Recto**  
2. Install → Enable

### Manual (GitHub Release)

1. Download `main.js`, `manifest.json`, and `styles.css` from the [latest Release](https://github.com/jensen-zheng-cmd/Recto-plugin/releases)  
2. Put them in `<vault>/.obsidian/plugins/recto/`  
3. Enable **Recto** under Community plugins

分发包**只有**上述三件套；不要把本机的 `data.json` 或开发文档拷进插件目录。

## Usage (short) / 使用简述

1. Enable the plugin → open **Recto Hub** from the ribbon or command palette.  
2. Sign in via the account flow (browser).  
3. Point Recto at your Zotero storage (auto-detect when possible).  
4. Pick papers in Hub → convert / translate → read the Markdown (and bilingual view) in your vault.

1. 启用插件 → 从侧栏或命令面板打开 **Recto Hub**。  
2. 按提示在浏览器完成登录。  
3. 配置 Zotero storage（可自动检测）。  
4. 在 Hub 选择论文 → 转换 / 翻译 → 在库内阅读 Markdown（及双语对照）。

## Development note / 开发说明

This public repository is the **distribution surface** for Community listing: root `README.md`, `LICENSE`, `manifest.json`, plus Release assets `main.js` / `manifest.json` / `styles.css`. Day-to-day development may happen in a separate private trunk; only the files needed to install and review the plugin are published here.

本公开仓库是社区上架用的**发布面**：根目录 `README.md`、`LICENSE`、`manifest.json`，以及 Release 附件三件套。日常开发可能在私有主干进行；此处只发布安装与审核所需文件。

## Third-party fonts / 第三方字体

Embedded UI fonts are licensed under the **SIL Open Font License 1.1**:

- [Inter](https://github.com/rsms/inter)  
- [Source Han Sans](https://github.com/adobe-fonts/source-han-sans) (SC subset; Reserved Font Name “Source”)  
- [JetBrains Mono](https://github.com/JetBrains/JetBrainsMono)

See <https://openfontlicense.org>.

## License / 许可

[MIT](./LICENSE) © 2026 Jensen Zheng

## Author / 作者

Jensen Zheng — GitHub [@jensen-zheng-cmd](https://github.com/jensen-zheng-cmd)

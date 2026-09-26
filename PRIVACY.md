# 隐私与数据说明 / Privacy and data

Honest disclosure for Obsidian Community review and for users. 与真实行为一致，不美化。

最后更新 / Last updated: 2026-09-26

## 什么会离开本机 / What leaves your computer

- When you start a **conversion**, selected **PDF files** are uploaded to the **Recto backend**. To
  finish parsing, summary, and translation, that content **may be forwarded to third-party
  processing services** chosen by Recto (vendor names intentionally omitted here; they can change as
  the service evolves).
  你发起**转换**时，所选 **PDF** 会上传到 **Recto 后端**；为完成解析、摘要与翻译，内容**可能转发至
  Recto 选用的第三方处理服务**（此处不列厂商名；服务选型可能随运营调整）。
- When you start **translation-only**, the plugin uploads the paper's **Sidecar** (structured text
  derived from the earlier parse), not the PDF again. That payload may likewise be processed by
  third-party services via the Recto backend.
  **只翻译**时上传的是该篇的 **Sidecar**（此前解析得到的结构化文本），不再传 PDF；同样可能经 Recto
  后端交由第三方处理。
- Account, billing, and session traffic also goes to Recto (`api.rectoai.uk` / `rectoai.uk`),
  including email for auth and payment handoff pages.
  账号、计费与会话流量也走 Recto（含认证邮件与支付交接页）。

- When you choose to **translate an existing Markdown file**, its text is sent to Recto as
  structured content for translation and may be processed by third-party services. Choosing to
  translate one file does not upload the rest of your vault.
  主动选择**翻译已有 Markdown 文件**时，该文件文本会以结构化内容发送至 Recto，并可能交由第三方
  服务处理。选择一个文件不会上传 vault 中的其他笔记。

## 什么留在本地 / What stays local

- Your vault stores generated Markdown / images / Sidecar, `papers.jsonl`, and plugin settings
  (`data.json` on disk). Vault notes are not uploaded as a whole; text you explicitly select for
  cloud translation is sent as described above.
  生成的 Markdown / 图片 / Sidecar、`papers.jsonl` 与插件设置（磁盘上的 `data.json`）保存在本机。
  不会整库上传笔记；您主动选择云端翻译的文本按上述方式发送。
- **Zotero**: Recto may read your local Zotero database and **storage folder outside the vault**
  (read-only import). It does not upload your whole Zotero library; only PDFs (or Sidecars) you
  explicitly queue for cloud processing are uploaded.
  **Zotero**：Recto 可能读取 vault **之外**的本地 Zotero 数据库与 **storage**（只读导入）。不会整库
  上传；只有你明确加入云端处理队列的 PDF（或 Sidecar）才会上传。
- Recto does **not** ship client-side telemetry, ads, or a self-update channel separate from
  Obsidian's normal Community Plugin updates.
  插件**不做**客户端遥测、动态广告，也不在 Obsidian 社区更新机制之外另做自我更新。

## 服务观测与反馈 / Service analytics and feedback

- The backend keeps operational metadata needed for billing, delivery, reliability, and support,
  such as task source/type, status, page or character counts, credit usage, processing timestamps,
  and error codes. This is **server-side service data**, not client-side behavioral telemetry.
  后端会保留计费、交付、可靠性与支持所需的服务元数据，例如任务来源/类型、状态、页数或字符量、
  额度用量、处理时间与错误码。这是**服务端服务数据**，不是客户端行为遥测。
- The in-plugin feedback form stores only the submitting account, feedback category, message, and
  submission time that the user actively submits. Feedback does **not**
  automatically attach PDFs, paper text, vault paths, client logs, passwords, payment credentials,
  or session tokens.
  插件内反馈表只保存用户主动提交时的登录账号、反馈类型、说明与提交时间；反馈**不会
  自动附带** PDF、论文正文、Vault 路径、客户端日志、密码、支付凭据或会话 token。

## 保留策略 / Retention

- **Task files** (uploaded PDFs, intermediates, result packages) are temporary. After the plugin
  acknowledges a successful write-back they are deleted promptly; if not pulled, results expire on a
  **24-hour** TTL. Failed-task files are kept briefly for retry, then purged (also on a **24-hour**
  window). Cancel deletes server-side task files immediately.
  **任务文件**（上传的 PDF、中间产物、结果包）为临时数据：插件确认写回成功后尽快删除；未拉取的结果约
  **24 小时**过期。失败任务文件短暂保留供重试后清理（同样约 **24 小时**）。取消任务会立即删除服务端
  任务文件。
- **Account records** (email, membership, credit ledger, orders) are kept so billing and support
  remain consistent. There is currently **no self-service "delete my account" button** in the
  product; contact the author if you need account closure.
  **账号记录**（邮箱、会员、额度账本、订单）会保留以便计费与支持。产品目前**没有自助销户入口**；
  如需关闭账号请联系作者。
- **Feedback records** are retained while they remain useful for resolving the report and improving
  the service, and are deleted if the linked account is deleted. Do not paste paper content or
  secrets into the feedback form; use the contact information shown in the plugin to request deletion
  or account closure.
  **反馈记录**会在处理问题与改进服务仍有需要时保留。请勿把论文内容或秘密粘贴进反馈表单；如需删除
  反馈或关闭账号，请使用插件内展示的联系方式；关联账号删除时，反馈记录一并删除。
- Payment processing uses third-party payment rails through Recto's checkout pages; Recto does not
  ask the plugin to store card numbers.
  支付经 Recto 结账页走第三方支付通道；插件不采集或存储银行卡号。

## 账号与登录 / Account and sign-in

- Sign-up and sign-in happen in the browser on the Recto account site; the plugin never asks you to
  type a password into Obsidian.
  注册与登录在浏览器账号页完成；插件不会在 Obsidian 内收集密码。
- New accounts need **email verification** before the first Obsidian session.
  新账号须**先验邮箱**才能进入 Obsidian 会话。
- Browsing a local library you already built does not require being signed in; conversion, summary
  and translation do.
  浏览你已落在本地的论文库不强制登录；转换、摘要与翻译需要登录。

## 联系 / Contact

插件内「问题反馈」会展示当前公开联系方式；也可通过 GitHub
[@jensen-zheng-cmd](https://github.com/jensen-zheng-cmd) 的公开仓库 Issues 联系作者。

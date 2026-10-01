# 隐私与数据说明 / Privacy and data

Honest disclosure for Obsidian Community review and for users. 与真实行为一致，不美化。

最后更新 / Last updated: 2026-10-01

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

## 操作诊断 / Operation diagnostics

When you use conversion, translation or recovery, Recto records operation starts, stages and outcomes,
including local checks that stop a request before submission. After cloud processing is enabled and
while signed in, it silently sends structured diagnostics to Recto for support. Reports contain random
operation/request IDs, the associated task ID, plugin/app versions, plugin source hash, operating system, timing, numeric sizes,
stable error codes and code locations/cause codes. They do not contain document text, PDF copies,
filenames, local paths, passwords/tokens, arbitrary exception messages or HTTP bodies/headers.
They are not forwarded to processing providers and do not track reading or browsing activity.

使用转换、翻译或恢复时，Recto 记录操作开始、阶段及结果，包括提交请求前的本地拦截。
启用云端处理且登录后，插件静默向 Recto 上传结构化诊断，供排障使用；包括随机操作/请求编号、
关联任务编号、插件/应用版本、插件源码哈希、操作系统、耗时、数字量级、稳定错误码、代码位置及原因链错误码。
诊断不含正文、PDF 副本、文件名、本地路径、密码/凭据、任意异常消息或请求/响应内容，
不转发至处理供应商，不跟踪阅读或浏览行为。

Pending reports are stored separately in the plugin directory (at most 250 events / 1 MiB).
Network failures are retried with backoff across restarts. Reports are bound to the original account;
signing into another account cannot upload them as that account. Reports created without an account
remain local. Local reports expire after 30 days; server reports become unavailable after 30 days
and are physically deleted by an hourly sweep. Only administrators can query them. A full queue may
discard its oldest events and record a gap count; corrupt queue contents are reset and the reset count
is recorded. An unreadable disk or permanent offline state can
prevent delivery. Contact the author using the details below for diagnostic deletion requests.
Reports that expire before confirmed delivery increment a persistent expiry gap count, included
in subsequent reports; expiry cannot establish whether a server received a report without its acknowledgment.

待传报告独立保存在插件目录，最多 250 条 / 1 MiB；断网时退避重传，重启后继续。
报告绑定原账号，换账号不会替原账号上传；未登录时产生的报告只留本地。
本地报告 30 天过期；服务端报告 30 天后不可查询，由每小时扫描物理删除，仅管理员可查。
队列满时可能丢弃最早报告并记录缺口计数，磁盘不可写或永久离线会阻止送达。
损坏的队列内容会被重建并记录重建次数。
送达未确认即过期的报告会累计持久化缺口计数，随以后的报告上传；缺少确认不代表服务器一定没收到。
如需删除诊断，可通过下方联系方式联系作者。

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
- Recto does **not** collect general usage analytics or ship ads or a self-update channel separate from
  Obsidian's normal Community Plugin updates.
  插件不采集一般使用行为统计、不展示动态广告；上面的操作诊断仅供排障。

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

- **Task files** (uploaded PDFs or text/structure, intermediates and result packages) remain
  available for **24 hours from completion or failure**, including after the plugin acknowledges
  local writeback. Receipt does not extend the window. Files are private; authenticated administrators
  may download them within that window for support verification, with an audit record and stated reason.
  At expiry, all task-file endpoints deny access immediately. A scheduled sweep deletes the objects
  and historical versions, retries failed deletion, and uses a terminal-only storage lifecycle as a
  backstop. Physical deletion may lag access expiry during outages. Cancellation requests immediate
  deletion with retries; uploads never started expire after 24 hours. Active processing is not cleaned
  up based on upload age. A retry inside the failure window starts a new attempt and a new window
  when that attempt ends. Task outcome, receipt and file cleanup are separate metadata; diagnostic
  reports retain their independent **30-day** window. This is temporary verification, not a backup.
  **任务文件**（上传的 PDF 或文本结构、中间产物和结果包）从**完成或失败时起保留 24 小时**，
  插件确认本地写回后仍保留；领取不延长窗口。文件私有，管理员可凭鉴权在窗口内下载核验，须填写理由并记录审计。
  到期立即禁止任务文件接口访问，应用定时删除对象及历史版本，失败重试，终态文件生命周期兜底；故障期间物理删除可能延迟。
  取消请求立即清理并重试；未启动的上传任务 24 小时后过期。在途处理不按上传年龄清理。
  在失败窗口内重试会进入新一轮，结束后重新计算保留窗口。处理结果、领取和清理分别记录；诊断仍独立保留 **30 天**。
  这是临时核验窗口，不是永久备份。

  Rollout: the T88-K backend migrations and private COS lifecycle rules went live on 2026-10-01.
  Acknowledgement now records receipt without immediately deleting the verification files.
  上线说明：T88-K 后端迁移及私有 COS 生命周期规则已于 2026-10-01 上线；领取只记录接收，不立即删除核验文件。
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

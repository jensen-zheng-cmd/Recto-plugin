# Recto

[English](https://github.com/jensen-zheng-cmd/Recto-plugin/blob/main/README.md) | **简体中文**

**将 Zotero 论文库迁入 Obsidian，保留书目信息、PDF 与分类结构，转换为 Markdown，并按所选语言翻译、对照阅读。借助 Obsidian 的双链、搜索与 AI 能力，让读过的论文彼此关联，逐步构建个人知识库。**

![Recto 论文库](assets/hub.png)

下方图片与动图沿用早期版本的中文界面演示。当前已支持简体中文与英文界面，部分按钮位置和操作方式有所调整。

## 背景

许多研究者的 Zotero 中积累了数百篇论文，但真正精读的屈指可数。阅读过程中，需要在 PDF 阅读器与翻译工具之间频繁切换；公式与表格经复制后往往格式混乱；已理解的内容散落在浏览器中，事后难以检索、引用，也无法纳入个人笔记体系。

Recto 旨在衔接这一工作流：将论文导入 Obsidian，转换为 Markdown 格式，提供翻译与对照阅读功能，所有产出均存储在本地库中，与既有笔记集成。

## 将 Zotero 论文库完整迁入 Obsidian

将书目元数据与本地 PDF 附件一并导入，保留 Zotero 分类层级与一篇论文属于多个分类的关系。标题、作者、出版信息、标签及其他书目字段随论文保留，导入后即可翻阅 PDF，再选择需要转换或翻译的论文。

从文献组织到阅读，论文库可以在 Obsidian 中继续使用。这里的迁入采用只读导入，不修改 Zotero 数据库，也不移除原文件；范围是已导入 PDF 对应的书目记录、元数据与分类，并非对所有 Zotero 条目类型、笔记和批注的完整备份。附件须已下载至本地，遇到多个 PDF 等歧义情况时由您确认选择。

![一键导入 Zotero](assets/zotero-import.gif)

![发起转换与翻译](assets/convert-translate.gif)

## 双语逐段对照，同步滚动

在设置中选择输出语言，译文按段落与原文关联，滚动时同步对照。阅读与对照跟随当前输出语言，其他语言的已有文件仍会保留。

界面语言与文档输出语言分别设置。界面可跟随 Obsidian，或手动选择简体中文、English；文档输出支持多种语言及自定义目标。这不代表所有语言或扫描 PDF 都能获得相同的 OCR 识别效果。

![原文译文双栏对照](assets/compare-md-md.gif)

## 译文与 PDF 原页联动

对某段翻译存疑时，在对照视图中点击该段落，PDF 即跳转至对应位置并高亮相关区域，方便核对措辞、公式与图表。

![PDF 与译文对照](assets/compare-md-pdf.gif)

## 保留公式、表格与插图

生成的 Markdown 文件中，公式以可渲染形式呈现，并保留表格与插图，而非仅提取纯文本。转换效果受原始 PDF 影响，复杂版式与扫描件仍可能需要对照原页核验。

![转换产物](assets/markdown-output.png)

## 文献管理与检索

沿用 Zotero 分类树结构，按处理状态筛选论文。单篇论文可标记为未读、在读、已读，支持按标题、作者、期刊、分类检索。后续检查 Zotero 更新时，可导入新论文、刷新分类信息，保留已有阅读产物。

![论文库筛选与阅读状态](assets/library-filter.gif)

## 生成结构化摘要

翻译论文库中的论文时，可选择同时生成摘要并设置详略。摘要跟随输出语言，单独保存为笔记，包含书目属性及回到论文的链接；已有摘要文件会保留。摘要是库内翻译时的可选项，不再随 PDF 转换自动生成。

![AI 摘要](assets/ai-summary.png)

## 安装

**通过社区插件市场安装**：设置 → 第三方插件 → 浏览 → 搜索 **Recto** → 安装并启用。

**手动安装**：前往 [Releases](https://github.com/jensen-zheng-cmd/Recto-plugin/releases) 下载 `main.js`、`manifest.json`、`styles.css` 三个文件，置于 `<your vault>/.obsidian/plugins/recto/` 目录下，重启 Obsidian 后启用。

## 使用步骤

1. **登录**。打开 Recto 设置，通过账号入口在浏览器中注册与登录，完成邮箱验证后返回 Obsidian。插件内不涉及密码输入。
2. **导入 Zotero**。按开始使用中的步骤定位本地 Zotero 数据目录，导入论文库。
3. **选择输出语言**。设置译文与新摘要所用的语言，它与界面语言相互独立。
4. **转换与翻译**。在论文列表中选中目标，通过详情栏发起转换或翻译；未转换的论文会先行转换再翻译。支持多选批量排队处理。
5. **阅读**。通过阅读主按钮及其下拉菜单打开已有文档；文件就绪后，使用对照入口打开原文/译文双栏或 PDF 对照。

论文库存储于设置指定的 vault 目录下，每篇论文对应一个子文件夹。新原文使用 `src-` 前缀，译文按目标语言使用 `zh-`、`zht-`、`en-`、`ja-` 等前缀，摘要使用 `br-`；旧版 `en-` 原文与 `ch-` 译文仍可阅读。

也可通过独立命令转换 Zotero 论文库之外的 PDF，或翻译已有 Markdown 文件。这两条路径不要求 Zotero，产物作为普通文件保存在 vault 中，不加入以 Zotero 分类组织的论文库；可选摘要仅适用于库内翻译。

## 使用前提与服务说明

- **仅支持 Obsidian 桌面端**。需读取本地文件系统，移动端不适用。
- **Zotero 导入需要本地 PDF 附件**。仅云端存储、尚未下载的附件须先下载；独立 PDF 转换与 Markdown 翻译不要求 Zotero。
- **云端处理需注册免费 Recto 账号并联网**。PDF 转换当前免费，翻译按页消耗额度，购买的额度永久有效。注册或邀请赠送以账号页展示为准；库内翻译时选取摘要不额外扣除翻译页数。
- **当前支付方式为微信支付，人民币计价**。海外银行卡支付与 Google 登录尚未开放，请使用邮箱注册与登录。
- **对照阅读需要对应文件就绪**。原文/译文对照需两份文档，PDF 对照还需 PDF 与定位数据；独立 PDF 转换时，如需对照，请开启保留 PDF 对照所需文件的选项。

## 网络服务说明

转换、摘要与翻译任务均提交至 **Recto 云端服务**（`api.rectoai.uk`）处理。转换上传所选 PDF；翻译上传结构化文本，翻译已有 Markdown 时会发送该文件的文本。账号与额度管理亦通过 Recto 服务。数据流转方式、留存时长及本地保留范围，详见 [隐私与数据说明](https://github.com/jensen-zheng-cmd/Recto-plugin/blob/main/PRIVACY.md)。

## 开发说明

本仓库为社区插件市场的发布页面，包含中英文 README、隐私说明、许可、媒体素材及分发所需的三个文件，安装文件附于 Release。日常开发在独立的私有仓库进行。

## 第三方字体

界面内嵌字体遵循 **SIL Open Font License 1.1** 授权：

- [Inter](https://github.com/rsms/inter)
- [Source Han Sans](https://github.com/adobe-fonts/source-han-sans)（SC 子集，保留字体名 "Source"）
- [JetBrains Mono](https://github.com/JetBrains/JetBrainsMono)

许可原文见 [SIL Open Font License](https://openfontlicense.org)。

## 许可

[MIT](https://github.com/jensen-zheng-cmd/Recto-plugin/blob/main/LICENSE) © 2026 Jensen Zheng

## 作者

Jensen Zheng — GitHub [@jensen-zheng-cmd](https://github.com/jensen-zheng-cmd)

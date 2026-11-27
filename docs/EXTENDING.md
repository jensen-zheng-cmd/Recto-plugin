# Extending Recto

Recto (RectoPaper on the Community store) turns a paper library into Obsidian files: PDFs, Markdown full text, translations, summaries, a machine-readable index and a reading hub. This document is the **public contract** for building on top of it — with Dataview / Bases, Templater or QuickAdd scripts, AI agents, or your own Obsidian plugin.

Everything listed here is stable within its major version: we only **add** fields, keys, methods and events. Removing or changing something listed here is a breaking change: it gets a new major version, is announced in the release notes, and the old form keeps working for at least one release with a console warning. Anything **not** listed here (CSS class names, internal methods, `data.json`, file contents beyond what is described) is internal and may change in any release.

Contract versions: **paper record schema 1**, **plugin API 1.1.0**. A contract test in Recto's development repository checks this document against the code on every change, so what you read here is what the plugin does.

## 1. Who owns what

Recto updates never touch files you own. Files Recto owns may be regenerated, so edits you make there can be lost.

| Owner | What | On Recto updates and re-runs |
|---|---|---|
| **Recto** | `papers.jsonl`; paper artifacts (`src-` / `en-` / `<language>-` / `ch-` / `br-` Markdown, the paper PDF copy, images); the `recto/` folder inside each paper folder; `^rc-` block anchors; `recto-*` frontmatter keys | Regenerated as needed. Do not edit; read them. |
| **You** | Each paper's note file `note-<stem>.md`; the note template `paper-note-template.md` (section 7); the library `AGENTS.md` once it exists (Recto only creates it when missing); everything else in your vault; your CSS snippets, templates, scripts and plugins | Never modified by Recto. |

Put your own notes, tags and properties in `note-<stem>.md` (or anywhere else you like), not in Recto's artifacts.

## 2. Library layout

The library lives in one vault folder (setting: *paper library folder*; new installs default to `Recto Library`, or `Recto 论文库` when the Recto interface is Chinese; existing installs keep the folder they already use, and the old default was `论文库`). Paths inside `papers.jsonl` are **relative to that folder**.

```
<library>/
  papers.jsonl                 machine index, one paper per line (section 3)
  AGENTS.md                    guide for AI agents (yours after creation)
  paper-note-template.md       your note template (optional, section 7)
  _recto-intake/               file hand-off for agents that add papers
  <stem>/                      one folder per paper
    <stem>.pdf                 PDF copy
    src-<stem>.md              source full text (older papers: en-<stem>.md)
    <language>-<stem>.md       translations, e.g. zh-<stem>.md (older papers: ch-<stem>.md)
    br-<stem>.md               AI summary (optional)
    note-<stem>.md             your note (yours)
    recto/sidecar-v1.json      structural map of the converted document
    recto/evidence-v2.json.gz  parser evidence snapshot
```

`<stem>` is the paper's folder and file name stem (naming rule *Author Year - Title* by default). It only changes through Recto's rename action; use `record_id` as the stable identity, never the stem.

## 3. `papers.jsonl` — paper record schema 1

UTF-8, one JSON object per line, sorted by original title. Recto rewrites the whole file whenever the library changes. A field that has no value is `null` (or `[]` for lists). The same records are returned by the plugin API (section 5).

| Field | Type | Meaning |
|---|---|---|
| `record_id` | string | Stable identity of the paper. Never changes, including across renames. |
| `source` | `"local"` \| absent | Present only for papers added to Recto without Zotero. |
| `zotero_key` | string \| null | Zotero item key (`null` for local papers). |
| `zotero_attachment_key` | string \| null | Zotero attachment key of the PDF. |
| `title_original` | string \| null | Original title (Zotero is authoritative). Never rewritten; sort and deduplicate by this. |
| `title_zh` | string \| null | Translated title taken from the Chinese translation, if any. Display `title_zh ?? title_original`. |
| `cite_label` | string \| null | Short citation label, e.g. `Smith et al. 2020`. |
| `citekey` | string \| null | Zotero / Better BibTeX citation key. |
| `csl` | object \| null | Standard CSL-JSON item (`id` is the citation key: the Better BibTeX key when present, otherwise generated; unique across the library and stable once assigned). Use it for bibliographies and exports. |
| `authors` | string[] | Authors in order. |
| `year` | string \| null | Publication year (from Zotero). |
| `venue` | string \| null | Journal, conference or publisher. |
| `category` | string \| null | Category from the AI summary. |
| `collections` | string[] | Collection paths as shown in Recto, e.g. `Methods / Optimization`. |
| `keywords` | string[] | Keywords from the AI summary (not Zotero tags). |
| `zotero` | object \| null | Zotero metadata, see below. |
| `summary_brief` | string \| null | One-paragraph summary from `br-<stem>.md`. |
| `summary_path` | string \| null | Summary file. |
| `ch_path` | string \| null | Chinese translation file. |
| `source_path` | string \| null | Source full-text file. |
| `pdf_path` | string \| null | PDF copy inside the paper folder. |
| `zotero_pdf_path` | string \| null | Origin of the PDF inside Zotero storage (`<attachmentKey>/<file>`), informational only. |
| `conversion_status` | `"converted"` \| `"unconverted"` | Whether Markdown full text exists. |
| `reading_status` | `"unread"` \| `"reading"` \| `"read"` | Reading status set in Recto. |
| `translations` | object[] \| absent | All translations of a converted paper, see below. |

`zotero` object:

| Field | Type | Meaning |
|---|---|---|
| `item_type` | string \| null | Zotero item type. |
| `venue` | string \| null | Normalised venue. |
| `creators` | object[] | `{ creator_type, name }` in order. |
| `tags` | object[] | `{ name, type }` Zotero tags. |
| `fields` | object | Zotero fields that exist on the item, under Zotero's own names (`DOI`, `abstractNote`, …). |

`translations[]` items:

| Field | Type | Meaning |
|---|---|---|
| `language` | string | Target language id, e.g. `zh-Hans`. |
| `language_name` | string | Display name of the language. |
| `path` | string | Translation file. |
| `document_id` | string | Document identity shared with the source file. |
| `source_revision_id` | string | Source revision the translation was made from. |
| `quality` | object \| null | `{ status: "complete" \| "degraded", fallbackBlockCount }` — `degraded` means some blocks kept the source text. |
| `source_version_matches` | boolean | `false` when the source text changed after this translation. |

## 4. Frontmatter keys in Recto artifacts (read-only)

Recto writes these keys into the frontmatter of the files it generates. Read them freely (Dataview, Bases, `metadataCache`); do not write them.

| Key | In | Meaning |
|---|---|---|
| `recto-document-id` | source, translations | Document identity. |
| `recto-source-revision-id` | source, translations | Revision of the source text. |
| `recto-sidecar` | source | Path of the paper's `sidecar-v1.json`. |
| `recto-artifact-role` | source, translations | `source` or `translation`. |
| `recto-naming-version` | source, translations | File naming scheme version (`1` = `src-` / `<language>-` names). |
| `recto-source-language` | source | Detected source language id. |
| `recto-source-language-evidence` | source | How the source language was determined. |
| `recto-translation-language` | translations | Target language id. |
| `recto-translation-alignment` | translations | Alignment ruleset the translation was checked against. |
| `recto-source-path` | translations | Vault path of the source the translation belongs to. |
| `recto-source-hash` | translations | Hash of the source text the translation was made from. |

Lines ending in a `^rc-NNNNNN` block anchor are Recto's alignment anchors between source and translation. You can link to them (`[[file#^rc-000012]]`); do not rename or remove them.

## 5. Plugin API 1.1.0

```js
const recto = this.app.plugins.plugins.recto?.api;   // undefined if Recto is not enabled
```

Recto fires `recto:api-ready` (with the api as argument) on the workspace when it finishes loading, and `recto:api-unload` when it unloads. After an unload the old reference stops working (`isActive()` is `false`) and everything you registered is gone; take a fresh reference on the next `recto:api-ready` and register again. Check `version` if you rely on members added after 1.0.0.

| Member | Returns | Notes |
|---|---|---|
| `version` | string | API version, semver. |
| `schemaVersion` | number | Paper record schema version (section 3). |
| `events` | string[] | Event names accepted by `on()`. |
| `isActive()` | boolean | `false` after Recto unloaded or reloaded. |
| `getLibraryFolder()` | string \| null | Vault path of the library folder; `null` if the setting is invalid. |
| `getPapers()` | object[] | All paper records (section 3). Computed on each call — cache it and refresh on `library-changed`. |
| `getPaper(recordId)` | object \| null | One paper record. |
| `resolveLibraryPath(relativePath)` | string \| null | Turns a record path (e.g. `source_path`) into a vault path for `app.vault.getAbstractFileByPath`. |
| `on(event, callback)` | function | Subscribes to an event; call the returned function to unsubscribe (do it in your plugin's `onunload`). |
| `getPaperForPath(vaultPath)` | object \| null | 1.1.0. The paper whose folder contains this vault file (source, translation, PDF, note, images…); `null` for other files. |
| `openPaper(recordId, target)` | Promise\<boolean\> | 1.1.0. Opens one of the paper's files, reusing a tab that already shows it. `target`: `source`, `translation`, `pdf`, `summary` or `note`. Never creates a file; `false` when it does not exist. |
| `revealInHub(recordId)` | Promise\<boolean\> | 1.1.0. Opens the Hub, clears filters that hide the paper, selects it and scrolls to it. |
| `getHubSelection()` | string[] | 1.1.0. `record_id`s of the papers selected in the Hub; `[]` when the Hub is closed. |
| `registerPaperMenuItem(options)` | function | 1.1.0. Adds an item to the Hub's paper context menu. Returns the function that removes it. |
| `registerPaperDetailSection(options)` | function | 1.1.0. Adds a section to the Hub's paper detail pane. Returns the function that removes it. |

Returned objects are copies; changing them changes nothing in Recto.

### Event `library-changed`

Fired after `papers.jsonl` is rewritten with different content — papers imported, added, converted, translated, renamed, recategorised, deleted, or their reading status changed.

```js
{
  added:   ["<record_id>", …],
  removed: ["<record_id>", …],
  changed: [{ recordId: "<record_id>", fields: ["conversion_status", "source_path", …] }, …],
  path:    "<library>/papers.jsonl"
}
```

`fields` lists the record fields that differ, so you can tell a finished conversion (`conversion_status`) from a new translation (`translations`) or a reading-status change (`reading_status`).

### Event `hub-selection-changed`

1.1.0. Fired when the papers selected in the Hub change, with `{ recordIds: ["<record_id>", …] }` (several when the user multi-selects, `[]` when nothing is selected or the Hub closes). Use it for a side panel that follows the Hub.

### Extension points

Both `register…` methods take an options object with an `id`. Use `<your-plugin-id>.<name>` (letters, digits, `.`, `_`, `:`, `-`; at most 80 characters). Registering an id twice throws; remove the old one first. Call the returned function in your plugin's `onunload`. Recto removes everything when it unloads, so register again on `recto:api-ready`.

Recto isolates your code. If a callback throws or its promise rejects, Recto logs it to the developer console under your id, and the rest of the Hub keeps working.

**`registerPaperMenuItem({ id, title, icon?, when?, onClick })`**

| Option | Type | Meaning |
|---|---|---|
| `id` | string | Unique id. |
| `title` | string | Menu item text. |
| `icon` | string | Optional [Lucide](https://lucide.dev) icon name, as in Obsidian. |
| `when` | function | Optional `papers => boolean`, called synchronously when the menu opens. The item shows only when it returns `true`. |
| `onClick` | function | `papers => void \| Promise`. On failure Recto shows a short error notice. |

`papers` are the records (section 3) of the papers the menu is for — one, or all selected papers. Extension items form their own group, above Recto's rename and delete items.

**`registerPaperDetailSection({ id, title?, render })`**

| Option | Type | Meaning |
|---|---|---|
| `id` | string | Unique id; the section element gets `data-recto-extension="<id>"`. |
| `title` | string | Optional heading, styled like Recto's own section headings. |
| `render` | function | `(el, paper, context) => void \| Promise`. Fill `el` with your content. |

Sections appear at the bottom of the detail pane while one paper is shown, in registration order. Recto redraws the detail pane often (selection changes, background reloads) and calls `render` each time with a new, empty `el`. So keep it fast, and cache what you fetch. For async work, check `context.isCurrent()` before writing into `el`: it is `false` once the pane was redrawn. If `render` fails, the section shows one error line.

For actions on files (file menu, editor menu, view header buttons), use Obsidian's own APIs (`file-menu`, `editor-menu`, `view.addAction`). Then use `getPaperForPath` to find the paper.

### Example

```js
const { Plugin, Notice } = require("obsidian");

module.exports = class MyPaperHelper extends Plugin {
  onload() {
    const attach = recto => {
      this.detach();
      const off = [];
      off.push(recto.on("library-changed", ({ changed }) => {
        for (const { recordId, fields } of changed) {
          if (fields.includes("conversion_status")) console.log("Converted:", recto.getPaper(recordId)?.title_original);
        }
      }));
      off.push(recto.registerPaperMenuItem({
        id: "my-paper-helper.copy-doi",
        title: "Copy DOI",
        icon: "clipboard",
        when: papers => papers.every(paper => paper.zotero?.fields?.DOI),
        onClick: async papers => {
          await navigator.clipboard.writeText(papers.map(paper => paper.zotero.fields.DOI).join("\n"));
          new Notice(`Copied ${papers.length} DOI(s)`);
        },
      }));
      off.push(recto.registerPaperDetailSection({
        id: "my-paper-helper.links",
        title: "Links",
        render: (el, paper) => {
          el.createEl("a", { text: "Open source text" }).onclick = () => recto.openPaper(paper.record_id, "source");
        },
      }));
      this.detach = () => off.splice(0).forEach(fn => fn());
    };
    this.detach = () => {};
    const ready = this.app.plugins.plugins.recto?.api;
    if (ready?.isActive()) attach(ready);
    this.registerEvent(this.app.workspace.on("recto:api-ready", attach));
    this.registerEvent(this.app.workspace.on("recto:api-unload", () => { this.detach = () => {}; }));
  }
  onunload() {
    this.detach();
  }
};
```

### What the API does not do

No writes, no conversion or translation, no account, credit or session access. Those stay inside Recto, behind its own confirmations. Menu items and detail sections run your code; they do not give it any extra access to Recto. If you need an action that is not here, open an issue on the public repository and describe the use case.

## 6. Styling with CSS snippets

Recto's panels and reader follow your Obsidian theme. To restyle them, add a CSS snippet (*Settings → Appearance → CSS snippets*). Snippets load after plugin styles, so a rule with the same selector as below wins. Only the variables, selectors and attributes in this section are stable; Recto's class names are internal.

### Interface variables

Set on `.recto-ui`, which is the root class of every Recto panel (Hub, settings, dialogs).

| Variable | Meaning |
|---|---|
| `--rc-accent` | Accent colour for buttons, selection and focus. By default it follows the *interface accent* setting. |
| `--rc-status-read` | Reading-status colour: read. |
| `--rc-status-reading` | Reading-status colour: reading. |
| `--rc-status-unread` | Reading-status colour: unread. |
| `--rc-success` | Semantic colour: success. |
| `--rc-warning` | Semantic colour: warning. |
| `--rc-danger` | Semantic colour: danger (delete, errors). |
| `--rc-info` | Semantic colour: information. |
| `--rc-font-ui` | Font stack of panel text. |
| `--rc-font-heading` | Font stack of panel headings. |
| `--rc-radius-s` | Small corner radius (buttons, chips). |
| `--rc-radius-m` | Medium corner radius (cards, menus). |
| `--rc-radius-l` | Large corner radius (dialogs). |

### Reader variables

Recto styles a paper's Markdown views with the class `.recto-reader` and the attribute `data-rc-theme`, whose value is the reader theme: `warm`, `latex`, `wenkai` or `night`. Override a variable for one theme with `.recto-reader[data-rc-theme="warm"]`, or for all themes with `.recto-reader[data-rc-theme]`.

| Variable | Meaning |
|---|---|
| `--rc-bg` | Page background. |
| `--rc-text` | Body text. |
| `--rc-heading` | Headings. |
| `--rc-muted` | Secondary text (captions, notes). |
| `--rc-accent` | Links, caret and accents. |
| `--rc-rule` | Rules and table borders. |
| `--rc-code-bg` | Code background. |
| `--rc-code-text` | Code text. |
| `--rc-thead-bg` | Table header background. |
| `--rc-selection` | Text selection. |
| `--rc-font-body` | Body font stack. |
| `--rc-para-space` | Space between paragraphs. |
| `--rc-indent` | First-line indent of paragraphs. |
| `--rc-align` | Paragraph alignment (`justify`, `start`). |
| `--rc-math-command` | Math source colour: commands. |
| `--rc-math-variable` | Math source colour: variables. |
| `--rc-math-operator` | Math source colour: operators. |
| `--rc-math-number` | Math source colour: numbers. |
| `--rc-math-text` | Math source colour: text. |
| `--rc-math-bracket-0` | Math source colour: brackets, level 1. |
| `--rc-math-bracket-1` | Math source colour: brackets, level 2. |
| `--rc-math-bracket-2` | Math source colour: brackets, level 3. |
| `--rc-math-delimiter` | Math source colour: delimiters. |

Reading width, line height and font scale come from Recto's settings. Recto sets them on the view itself (`--rc-measure`, `--rc-line-height`, `--rc-font-scale`), so you can read them in a snippet, but change them in the settings.

### Data attributes

| Attribute | On | Value |
|---|---|---|
| `data-recto-view` | Hub root | `hub` |
| `data-recto-record-id` | Hub list row; Hub detail pane while one paper is shown | The paper's `record_id`. |
| `data-recto-reading-status` | Hub list row | `unread`, `reading` or `read`. |
| `data-recto-conversion-status` | Hub list row | `converted` or `unconverted`. |
| `data-recto-extension` | Hub detail section added by a plugin (section 5) | The section's `id`. |

### Example

```css
/* A calmer accent in Recto panels */
.recto-ui { --rc-accent: #3b6ea8; }

/* Fade papers you have read */
[data-recto-view="hub"] [data-recto-reading-status="read"] { opacity: 0.6; }

/* Narrower paragraph spacing and no indent in the LaTeX reader theme */
.recto-reader[data-rc-theme="latex"] { --rc-para-space: 0.3em; --rc-indent: 0; }
```

## 7. Paper note template

When Recto creates a paper note (`note-<stem>.md`, from the Hub's note button), it uses the file `paper-note-template.md` in the library folder if it exists. Without it, Recto writes its built-in content: an empty note, or the paper's section outline. The template is your file; Recto only reads it, and never changes notes that already exist.

`{{name}}` inserts the value as text. Lists are joined with `, ` and missing values are empty. `{{name|json}}` inserts the value as JSON (`null` when missing), which is safe inside frontmatter. Placeholders that are not in the table below are left unchanged, so Templater (`<% … %>`) and other template syntax in the file keep working.

| Placeholder | Value |
|---|---|
| `record_id` | `record_id` (section 3). |
| `title` | `title_zh`, or `title_original` when there is no translated title. |
| `title_original` | `title_original`. |
| `title_zh` | `title_zh`. |
| `authors` | `authors` (list). |
| `year` | `year`. |
| `venue` | `venue`. |
| `citekey` | `citekey`. |
| `cite_label` | `cite_label`. |
| `category` | `category`. |
| `keywords` | `keywords` (list). |
| `collections` | `collections` (list). |
| `summary_brief` | `summary_brief`. |
| `reading_status` | `reading_status`. |
| `doi` | DOI from the Zotero fields. |
| `url` | URL from the Zotero fields. |
| `date` | Today's date, `YYYY-MM-DD`. |
| `pdf_link` | Link to the PDF copy. |
| `source_link` | Link to the source full text. |
| `translation_link` | Link to the translation in your target language (else any translation). |
| `summary_link` | Link to the AI summary. |
| `outline` | The section outline when the note style is *section outline*; empty for a blank note. With the outline style and no `{{outline}}` in the template, the outline is added at the end. |

Links follow your Obsidian link settings (wikilinks or Markdown links) and are empty when the file does not exist yet.

```markdown
---
title: {{title_original|json}}
authors: {{authors|json}}
year: {{year|json}}
citekey: {{citekey|json}}
tags: [paper]
---
# {{title}}

{{authors}} · {{venue}} · {{year}}
Read: {{translation_link}} · Original: {{source_link}} · PDF: {{pdf_link}}

## Outline
{{outline}}

## My notes
```

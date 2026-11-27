# Zotero PDF OCR and Translation - RectoPaper

**English** | [简体中文](https://github.com/jensen-zheng-cmd/Recto-plugin/blob/main/README.zh-CN.md)

**Bring your Zotero paper library into Obsidian, convert PDFs to Markdown, and read originals alongside translations in your chosen language. Connect papers with your notes through Obsidian's links, search, and AI tools to build a personal knowledge base.**

![Recto paper library](assets/hub.png)

Screenshots show an earlier Chinese interface. Recto supports English and Simplified Chinese.

## Background

Many researchers have hundreds of papers in Zotero but have read only a fraction closely. Reading often means switching between a PDF reader and a translation tool. Copied equations and tables lose their formatting, while useful passages remain scattered across browser tabs instead of becoming part of a searchable note collection.

Recto connects this workflow: bring papers into Obsidian, convert them to Markdown, translate them, and read them side by side. Generated files stay in your vault, where they can connect with your existing notes.

## Bring your Zotero paper library into Obsidian

Import bibliographic metadata and locally available PDF attachments together, preserving Zotero's collection hierarchy and papers that belong to multiple collections. Titles, authors, publication details, tags, and other bibliographic fields remain available in the local library. You can browse imported PDFs immediately, then choose which papers to convert or translate.

This brings the paper library's organization and reading workflow into Obsidian. Import reads Zotero without modifying its database or removing its files. It covers bibliographic records associated with imported PDFs, their metadata, and collections; it is not a full backup of every Zotero item type, note, or annotation. Attachments must be available locally. Ambiguous PDF choices are presented for confirmation.

Recto can also copy a paper's Zotero quick note on its first import. The optional [Recto Quick Note companion](https://github.com/jensen-zheng-cmd/Recto-plugin/releases/download/1.1.27/recto-quick-note-0.1.1.xpi) for Zotero 10.0.x adds an editable quick note below the authors in Zotero's Info pane. Write the note before importing the paper into Recto. Later syncs preserve the existing Recto quick note, including local edits or an empty value; notes added in Zotero afterward are not fetched. You can still edit the quick note in Recto.

![Import from Zotero](assets/zotero-import.gif)

![Convert and translate papers](assets/convert-translate.gif)

## Add papers without Zotero

Paste a DOI, arXiv ID, title, or paper page link into the library toolbar. Recto looks for a legal open-access full text and adds the paper to Recto, or to both Recto and Zotero. Papers without an open PDF wait under **Needs PDF**: drop in a PDF, or fetch it through the browser extension using your own browser sign-in.

## Read originals and translations side by side

Choose an output language in settings. Translations are linked to the original by paragraph, with synchronized scrolling for comparison. Reading and comparison follow the selected output language; existing files in other languages are preserved.

The interface language and document output language are independent. The interface can follow Obsidian or use English or Simplified Chinese. Document output supports multiple languages, including custom language choices; this does not imply equally reliable OCR for every language or scanned PDF.

![Side-by-side reading](assets/compare-md-md.gif)

## Compare translations with the original PDF

When a passage needs checking, click it in the comparison view to jump to its location in the PDF and highlight the corresponding region. This makes it easier to check wording, equations, and figures against the source page.

![PDF comparison](assets/compare-md-pdf.gif)

## Preserve equations, tables, and illustrations

Conversion produces Markdown with renderable equations, tables, and illustrations, rather than a plain stream of extracted text. Results depend on the source PDF; complex layouts and scans may still need checking against the original.

![Converted Markdown](assets/markdown-output.png)

## Organize and search your papers

Browse the Zotero collection tree, filter papers by processing status, and mark them as unread, reading, or read. Search by title, author, publication, or collection. Subsequent Zotero checks can bring in new papers and refresh collection information without replacing existing reading outputs.

Move papers between collections and create or rename collections without changing Zotero. A naming rule such as "Author Year - Title" can rename existing papers after a preview, and links in notes and Canvas follow. Drag a paper into a note or Canvas to insert a real link or file card.

![Library filters and reading status](assets/library-filter.gif)

## Generate structured summaries

When translating papers from the library, you can also request a summary and choose its level of detail. The summary follows your output language and is saved as a separate note, with bibliographic properties and links back to the paper. Existing summary files are preserved. Summaries are optional during library translation, not generated automatically by PDF conversion.

![AI summary](assets/ai-summary.png)

## Copy citations

Copy a reference in GB/T 7714, APA 7, IEEE, or MLA 9, or export BibTeX, RIS, or CSL-JSON. Journal names stay italic when pasted into Word. The detail panel flags missing fields such as volume or pages and can complete them from Crossref.

## Installation

**Community plugins**: Settings → Community plugins → Browse → search for **RectoPaper** → Install and enable. Or open the [plugin page](https://obsidian.md/plugins?id=recto) directly.

**Manual installation**: download `main.js`, `manifest.json`, and `styles.css` from [Releases](https://github.com/jensen-zheng-cmd/Recto-plugin/releases). Place them in `<your vault>/.obsidian/plugins/recto/`, restart Obsidian, and enable the plugin.

## Getting started

1. **Sign in.** Open Recto settings and follow the account link. Registration and sign-in take place in your browser; verify your email and return to Obsidian. Passwords are not entered inside the plugin.
2. **Import Zotero.** Follow the setup steps to locate the local Zotero data directory and import your paper library.
3. **Choose an output language.** Set the language for translations and new summaries independently of the interface language.
4. **Convert and translate.** Select papers in the library and use the detail panel to convert or translate them. Papers without Markdown are converted before translation. Multiple papers can be queued together.
5. **Read and compare.** Use the reading button and its menu to open an available document. The comparison controls open original/translation or PDF comparison views when the required files are ready.

The library lives in your configured vault folder, with a subfolder for each paper. New source Markdown uses `src-`; translated files use a language prefix such as `zh-`, `zht-`, `en-`, or `ja-`; summaries use `br-`. Older `en-` source files and `ch-` translations remain readable.

You can also convert PDFs outside the Zotero library or translate an existing Markdown file. These commands do not require Zotero and save ordinary files in the vault rather than adding entries to the Zotero-based library. The optional summary step applies only to library translation.

## Requirements and service access

- **Obsidian desktop only.** Recto reads the local filesystem; mobile is not supported.
- **Zotero library import requires local PDF attachments.** Cloud-only attachments must be downloaded first. Independent PDF conversion and Markdown translation do not require Zotero.
- **Cloud processing requires a free Recto account and an internet connection.** PDF conversion is currently free. Translation uses page credits; purchased credits do not expire. Any signup or invitation allowance is shown on the account page. An optional summary requested with library translation does not use additional translation pages.
- **Payments**: WeChat Pay (CNY) or card checkout via Paddle (USD). The default follows your region, and you can switch on the checkout page. Sign in with email, Google, or GitHub.
- **Comparison requires matching files.** Original/translation comparison needs both documents; PDF comparison also needs the PDF and its mapping data. For independent PDF conversion, enable the option to retain files for PDF comparison if needed.

## Network services

Conversion, summary, and translation requests go to the **Recto cloud service** (`api.rectoai.uk`). Conversion uploads the selected PDF. Translation uploads structured text; translating an existing Markdown file sends that file's text for processing. Account and credit management also use Recto's service. Requests to Recto carry the plugin version and interface language; for signed-in accounts the server keeps one daily activity row (date, plugin version, interface language) to measure retention, plus the invite code used at sign-up, if any. No document content or in-app behavior is recorded. For data handling, retention, and local storage details, see [Privacy and data](https://github.com/jensen-zheng-cmd/Recto-plugin/blob/main/PRIVACY.md).

Adding papers by DOI, arXiv ID, title, or link queries OpenAlex, Unpaywall, Crossref, Semantic Scholar, and arXiv directly from your device, and reads the publisher page for a pasted link. These lookups do not pass through Recto's servers, and paywalls are never bypassed.

## Development

This repository is the Community plugin release surface. It contains the English and Chinese READMEs, privacy notice, license, developer contract, media assets, and the three plugin distribution files, with installable assets attached to Releases. Development takes place in a separate private repository.

To build plugins, scripts, Dataview/Bases views, or AI-agent workflows on top of Recto, see [Extending Recto](docs/EXTENDING.md): the library layout, the `papers.jsonl` schema, the frontmatter keys, and the read-only plugin API with its `library-changed` event.

## Third-party fonts

Embedded interface fonts are licensed under the **SIL Open Font License 1.1**:

- [Inter](https://github.com/rsms/inter)
- [Source Han Sans](https://github.com/adobe-fonts/source-han-sans) (SC subset; reserved font name "Source")
- [JetBrains Mono](https://github.com/JetBrains/JetBrainsMono)

License text: [SIL Open Font License](https://openfontlicense.org).

## License

[MIT](https://github.com/jensen-zheng-cmd/Recto-plugin/blob/main/LICENSE) © 2026 Jensen Zheng

## Author

Jensen Zheng — GitHub [@jensen-zheng-cmd](https://github.com/jensen-zheng-cmd)

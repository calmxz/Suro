# Reading a material reliably

This file answers issue 4: which ways of reading a material work reliably, and where does each one fail?
It covers direct model input, OCR services, and parsing libraries.
It covers text PDFs, scanned PDFs, slides, phone photos, and handwriting.

All pricing and limits pages were read on 2026-11-26.
Prices are in USD.
Each table row and each claim links to the page that owns it.
"Not verified" means that no primary page confirmed the claim during this research.

## Short answer

- Direct model input is the most capable way to read a material.
  Claude, Gemini, and OpenAI each read a text PDF and a scanned PDF as text plus page images.
- Gemini and Claude publish per-page token counts, and OpenAI does not.
  Of the two, Gemini Flash-Lite is the cheapest.
- Gemini is the only one of the three with a documented API free plan.
  The free plan lets Google use the content to improve its products, and human reviewers can read it.
- Only Gemini 4 reads a PPTX file as slide images.
  Gemini 3 and 2.5 read the text and embedded images.
  OpenAI reads only the text of a PPTX file.
  The Claude PDF guide does not list PPTX, and it says to convert binary formats first.
- Only Gemini accepts HEIC and HEIF images.
  Claude and OpenAI accept JPEG, PNG, GIF, and WebP.
- Cloud OCR costs about $1.50 for each 1,000 pages after a small free allowance.
  Mistral OCR 3 costs $2.00.
- Handwriting is the weakest case for every option.
  None of the pages read gives accuracy figures for handwriting or lists Filipino handwriting support.
- Text-layer parsing libraries are free and exact on text PDFs.
  On a scanned PDF, they read only a text layer that the scanner added, with its OCR errors.
  Some libraries, such as Docling and PyMuPDF, can call an OCR engine.
- The license that matters most is AGPL-3.0, on PyMuPDF and `mupdf`.
  Most other libraries use MIT, BSD, or Apache-2.0.

## The three ways to read a material

1. **Direct model input.** You send the file to a multimodal model, and the model reads the text and the page images.
2. **OCR.** An OCR engine or service turns page images into text.
   You then send the text to the model.
3. **Parsing libraries.** A library reads the text that the file already contains.
   Examples are the text layer of a PDF and the text boxes of a PPTX file.

A parsing library is exact when the file has real text.
OCR is necessary when the file is only an image.
Direct model input can do both, and it can also read charts and diagrams.

## Direct model input

### Claude (Anthropic)

| Item | Value | Source |
| --- | --- | --- |
| How a PDF is read | Each page becomes an image, and the extracted text of each page goes with it. | [PDF support](https://platform.claude.com/docs/en/build-with-claude/pdf-support) |
| PDF pages per request | 600, or 100 when the context window is under 1M tokens | [PDF support](https://platform.claude.com/docs/en/build-with-claude/pdf-support) |
| Request size | 128 MB on the first-party API | [PDF support](https://platform.claude.com/docs/en/build-with-claude/pdf-support) |
| PDF format limit | Standard PDF, no passwords or encryption | [PDF support](https://platform.claude.com/docs/en/build-with-claude/pdf-support) |
| PDF token cost | About 1,500 to 3,000 text tokens per page, plus at most 1,568 image tokens per page | [PDF support](https://platform.claude.com/docs/en/build-with-claude/pdf-support) |
| Other file types | `.txt`, `.csv`, and `.md` work as plain text. `.docx` and `.xlsx` must be converted first. PPTX is not listed. | [PDF support](https://platform.claude.com/docs/en/build-with-claude/pdf-support) |
| Image formats | JPEG, PNG, GIF, WebP. Animated images use only the first frame. | [Vision](https://platform.claude.com/docs/en/build-with-claude/vision) |
| Image size | 10 MB per image (base64), 8000 x 8000 px maximum | [Vision](https://platform.claude.com/docs/en/build-with-claude/vision) |
| Images per request | 100 for 200k-context models, 600 for other models. Above 20 images, a stricter size limit applies; 3,000 px or less is safe. | [Vision](https://platform.claude.com/docs/en/build-with-claude/vision) |
| Image token cost | `ceil(width / 28) x ceil(height / 28)`. The cap is 4,784 tokens on Claude 4.7 and later, and 1,568 tokens on other models. | [Vision](https://platform.claude.com/docs/en/build-with-claude/vision) |
| Files API | 500 MB per file, 1 TB per organization. Upload is free. Content is billed as input tokens. | [Files API](https://platform.claude.com/docs/en/build-with-claude/files) |
| Context window | 1M tokens on Fable 5.2, Opus 5.5, Sonnet 5.5, and Haiku 5.5 | [Models overview](https://platform.claude.com/docs/en/about-claude/models/overview) |

Standard input prices per 1M tokens, from the [pricing page](https://platform.claude.com/docs/en/about-claude/pricing):

| Model | Input | Output | Cache hit |
| --- | --- | --- | --- |
| Haiku 5.5, prompt up to 100,000 tokens | $0.10 | $0.50 | $0.01 |
| Haiku 5.5, prompt over 100,000 tokens | $0.50 | $2.50 | $0.05 |
| Haiku 4.5 | $1.00 | $5.00 | $0.10 |
| Sonnet 5.5 | $2.00 | $10.00 | $0.10 |
| Opus 5.5 | $4.00 | $20.00 | $0.20 |

The Batch API halves input and output prices ([pricing](https://platform.claude.com/docs/en/about-claude/pricing)).
A Claude paid plan does not include API usage.
Max, Ultra, and Team plans include monthly API credits ([help center](https://support.claude.com/en/articles/9876003)).
The credit amount is Not verified.

Stated limitations ([Vision](https://platform.claude.com/docs/en/build-with-claude/vision)):

- Claude can make mistakes on low-quality, rotated, or very small images under 200 px.
- Resizing can make small text hard to read.
- Heavy JPEG compression can make text hard to read.
- The PDF guide asks you to rotate pages upright and to use clear, legible text ([PDF support](https://platform.claude.com/docs/en/build-with-claude/pdf-support)).

The pages do not mention handwriting.

### Gemini (Google)

| Item | Value | Source |
| --- | --- | --- |
| How a PDF is read | Pages render as 2-megapixel images at 288 DPI. The embedded text goes with them at no extra token charge. | [Document understanding](https://ai.google.dev/gemini-api/docs/document-processing) |
| File size and pages | 50 MB per file. 1,000 pages per PDF. 2,000 pages or slides per Office file. | [Document understanding](https://ai.google.dev/gemini-api/docs/document-processing) |
| PDF and slide tokens, Gemini 4 and 3 | About 280 (low), 560 (medium, default), or 1,120 (high) per page | [Document understanding](https://ai.google.dev/gemini-api/docs/document-processing), [Media resolution](https://ai.google.dev/gemini-api/docs/media-resolution) |
| PDF tokens, Gemini 2.5 | 258 per page | [Document understanding](https://ai.google.dev/gemini-api/docs/document-processing) |
| PPTX and DOCX, Gemini 4 | Read as page or slide images, about 560 tokens per page | [Document understanding](https://ai.google.dev/gemini-api/docs/document-processing) |
| PPTX and DOCX, Gemini 3 and 2.5 | Extracted text plus embedded images | [Document understanding](https://ai.google.dev/gemini-api/docs/document-processing) |
| Image formats | PNG, JPEG, WEBP, HEIC, HEIF | [Image understanding](https://ai.google.dev/gemini-api/docs/image-understanding) |
| Images per request | 3,600 | [Image understanding](https://ai.google.dev/gemini-api/docs/image-understanding) |
| Image tokens, Gemini 4 and 3 | 1,120 by default. 280, 560, 1,120, or 2,240 by setting. | [Media resolution](https://ai.google.dev/gemini-api/docs/media-resolution) |
| Inline data | 100 MB per request, 50 MB for PDFs | [File input methods](https://ai.google.dev/gemini-api/docs/file-input-methods) |
| Files API | 2 GB per file, 20 GB per project. Files expire after 48 hours. No cost. | [File input methods](https://ai.google.dev/gemini-api/docs/file-input-methods), [Document understanding](https://ai.google.dev/gemini-api/docs/document-processing) |

The image page also gives an older rule: 258 tokens per 768 x 768 tile.
The page does not say which model family uses that rule ([Image understanding](https://ai.google.dev/gemini-api/docs/image-understanding)).

Input prices per 1M tokens, from the [pricing page](https://ai.google.dev/gemini-api/docs/pricing):

| Model | Free plan | Paid input | Paid output |
| --- | --- | --- | --- |
| Gemini 3.1 Flash-Lite | Yes | $0.25 | Not recorded |
| Gemini 3.5 Flash-Lite | Yes | $0.30 | $2.50 |
| Gemini 3.8 Flash | Yes | $0.75 through 2026-12-31, then $1.50 | $3.75, then $7.50 |
| Gemini 4 Hydrogen | Yes | $1.50 | $7.50 |
| Gemini 4 Carbon, Argon, Neon | No | $2.00 to $3.00. Some prices change after 2026-11-30. | Not recorded |

Free plan facts:

- The Philippines is on the list of available regions ([available regions](https://ai.google.dev/gemini-api/docs/available-regions)).
- Rate limits apply per project, and the daily quota resets at midnight Pacific time ([rate limits](https://ai.google.dev/gemini-api/docs/rate-limits)).
- The docs do not publish free-plan requests per minute or per day for each model. The docs point to the AI Studio dashboard. The numbers are Not verified.
- The pricing page marks free-plan content as "Used to improve our products: Yes" ([pricing](https://ai.google.dev/gemini-api/docs/pricing)).

The terms say this about unpaid services ([Gemini API terms](https://ai.google.dev/gemini-api/terms)):

> To help with quality and improve our products, human reviewers may read, annotate, and process your API input and output.

> Do not submit sensitive, confidential, or personal information to the Unpaid Services.

The terms also contain a separate rule for the European Economic Area, Switzerland, and the United Kingdom.
This research does not interpret that rule.

Stated limitations ([Document understanding](https://ai.google.dev/gemini-api/docs/document-processing)):

- Rotate pages to the correct orientation before you send them.
- Avoid blurry scans and low-contrast pages.
- Plain text, Markdown, and HTML files are read as text only, without layout or charts.

The pages do not mention handwriting.

### OpenAI

| Item | Value | Source |
| --- | --- | --- |
| How a PDF is read | Text and page images, sent to a model that accepts images | [File inputs](https://developers.openai.com/api/docs/guides/file-inputs) |
| How DOCX and PPTX are read | Text only. Embedded images and charts are not extracted. Convert to PDF to keep charts. | [File inputs](https://developers.openai.com/api/docs/guides/file-inputs) |
| File size | Each file under 50 MB. 50 MB combined for all files in a request. | [File inputs](https://developers.openai.com/api/docs/guides/file-inputs) |
| PDF page limit | Not stated on the page. Not verified. | [File inputs](https://developers.openai.com/api/docs/guides/file-inputs) |
| PDF tokens per page | Not stated. The `detail` setting changes the image share. Not verified. | [File inputs](https://developers.openai.com/api/docs/guides/file-inputs) |
| Chat Completions | Accepts only PDF files | [File inputs](https://developers.openai.com/api/docs/guides/file-inputs) |
| Image formats | PNG, JPEG, WEBP, non-animated GIF | [Images and vision](https://developers.openai.com/api/docs/guides/images-vision) |
| Request size and count | 512 MB per request, 1,500 images per request | [Images and vision](https://developers.openai.com/api/docs/guides/images-vision) |
| Image tokens, `gpt-6-astra` at `high` | Up to 2,500 patches, times 1.2. A 1024 x 1024 image costs 1,229 tokens. | [Images and vision](https://developers.openai.com/api/docs/guides/images-vision) |

Selected input prices per 1M tokens, short context, from the [pricing page](https://developers.openai.com/api/docs/pricing):
`gpt-6-luna` $0.10, `gpt-5-nano` $0.05, `gpt-5-mini` $0.25, `gpt-6.1-terra` $1.00, `gpt-6.2-sol` $2.00.
Whether each of these models accepts images was Not verified.
An OpenAI free plan for the API was Not verified.

Stated limitations ([Images and vision](https://developers.openai.com/api/docs/guides/images-vision)):

- The model can fail on text in non-Latin alphabets.
- Small text needs enlarging, or the `original` detail level.
- The model can misread rotated or upside-down text.
- The model can struggle with charts whose line styles or colors vary.
- The model can struggle with panoramic and fisheye images.

The page does not mention handwriting.

### Estimated cost to read 1,000 PDF pages directly

These are estimates.
They count input only, for one read of each page.
Output tokens and your prompt are extra.

| Option | Tokens per page | Estimate per 1,000 pages |
| --- | --- | --- |
| Gemini 3.1 Flash-Lite, default resolution | about 560 | about $0.14 |
| Gemini 3.5 Flash-Lite, default resolution | about 560 | about $0.17 |
| Gemini 4 Hydrogen, default resolution | about 560 | about $0.84 |
| Gemini free plan | about 560 | $0, with the data-use terms above |
| Claude Haiku 5.5, prompt up to 100,000 tokens | about 3,000 to 4,600 | about $0.30 to $0.46 |
| Claude Haiku 5.5, prompt over 100,000 tokens | about 3,000 to 4,600 | about $1.50 to $2.30 |
| Claude Haiku 4.5 | about 3,000 to 4,600 | about $3.00 to $4.60 |
| Claude Sonnet 5.5 | about 3,000 to 4,600 | about $6.00 to $9.20 |
| OpenAI | Not published | Not verified |

The Gemini estimate assumes default processing, where embedded PDF text costs nothing extra.
If you set `processing` options, the media resolution table lists "560 + Native Text" per page ([Media resolution](https://ai.google.dev/gemini-api/docs/media-resolution)).
Then the text tokens add to the cost.
The Claude estimate uses 1,500 to 3,000 text tokens plus up to 1,568 image tokens per page.
At that rate, one request passes 100,000 tokens at about 21 to 33 pages.
Above that point, Haiku 5.5 costs five times more per token.

### Cost that repeats

Direct input is billed on every request that contains the material.
Claude resends the full conversation history on each turn ([Vision](https://platform.claude.com/docs/en/build-with-claude/vision)).
If the material stays in the history, you pay for it on every turn.
Prompt caching lowers the repeat cost to 0.1x the input price on most Claude models ([pricing](https://platform.claude.com/docs/en/about-claude/pricing)).
If you extract the text once and store it, you pay for the read once.
Files sent through the Gemini Files API expire after 48 hours, so you must send such a material again later ([File input methods](https://ai.google.dev/gemini-api/docs/file-input-methods)).

## OCR

### Local OCR engines

| Engine | License | Notes | Source |
| --- | --- | --- | --- |
| Tesseract | Apache-2.0 | More than 100 languages. The `tessdata` repo has `fil` (Filipino) and `tgl` (Tagalog) models. | [tesseract](https://github.com/tesseract-ocr/tesseract), [tessdata](https://github.com/tesseract-ocr/tessdata), [data files](https://tesseract-ocr.github.io/tessdoc/Data-Files-in-different-versions.html) |
| Tesseract.js | Apache-2.0 | Runs in the browser and in Node.js. It does not support PDF files. | [tesseract.js](https://github.com/naptha/tesseract.js) |
| Scribe.js | AGPL-3.0 | The tesseract.js README suggests it for PDF support. | [scribe.js](https://github.com/scribeocr/scribe.js) |
| EasyOCR | Apache-2.0 | Filipino support Not verified. | [EasyOCR](https://github.com/JaidedAI/EasyOCR) |
| PaddleOCR | Apache-2.0 | Filipino support Not verified. | [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR) |
| RapidOCR | Apache-2.0 | PyMuPDF recommends it over Tesseract for accuracy. | [RapidOCR](https://github.com/RapidAI/RapidOCR), [PyMuPDF README](https://github.com/pymupdf/PyMuPDF) |

Tesseract failure cases, from its own docs:

- Handwriting: "You can, but it won't work very well, as Tesseract is designed for printed text" ([FAQ](https://tesseract-ocr.github.io/tessdoc/FAQ.html)).
- Low resolution: Tesseract works best at 300 DPI or more ([Improve quality](https://tesseract-ocr.github.io/tessdoc/ImproveQuality.html)).
- Skew: line segmentation quality drops a lot on a skewed page ([Improve quality](https://tesseract-ocr.github.io/tessdoc/ImproveQuality.html)).
- Noise: some noise survives binarization and lowers accuracy ([Improve quality](https://tesseract-ocr.github.io/tessdoc/ImproveQuality.html)).

These cases match a phone photo of a page: often tilted, unevenly lit, and noisy.

### Cloud OCR and parsing services

| Service | Free allowance | Paid price | Limits | Source |
| --- | --- | --- | --- | --- |
| Google Cloud Vision, `DOCUMENT_TEXT_DETECTION` | First 1,000 units each month | $1.50 per 1,000 units | Each PDF page counts as one image | [pricing](https://cloud.google.com/vision/pricing) |
| Google Document AI, Enterprise Document OCR | First 1,000 pages. Monthly reset Not verified. | $1.50 per 1,000 pages | Layout Parser costs $10 per 1,000 pages | [pricing](https://cloud.google.com/document-ai/pricing) |
| Azure Document Intelligence, Read | 500 pages each month on F0 | $1.50 per 1,000 pages on S0 | F0: 4 MB per file and the first 2 pages per request. S0: 500 MB and 2,000 pages. | [pricing](https://azure.microsoft.com/en-us/pricing/details/ai-document-intelligence/), [retail price API](https://prices.azure.com/api/retail/prices), [service limits](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/service-limits) |
| AWS Textract, Detect Document Text | 1,000 pages each month for 3 months, new customers only | $1.50 per 1,000 pages | Not recorded | [pricing](https://aws.amazon.com/textract/pricing/) |
| Mistral OCR 3 | None for the API | $2 per 1,000 pages, $1 with Batch | 50 MB and 1,000 pages per file | [announcement](https://mistral.ai/news/mistral-ocr-3), [OCR docs](https://docs.mistral.ai/capabilities/document_ai/basic_ocr/), [tiers](https://docs.mistral.ai/deployment/laplateforme/tier/) |
| OCR.space | 25,000 requests each month, 500 per day per IP | PRO $30 per month for 300,000 requests | Free: 1 MB per file, 3 PDF pages | [OCR API](https://ocr.space/ocrapi) |
| LlamaParse | 10,000 credits each month | $1.50 per 1,000 credits | 1 to 25 credits per page by tier | [pricing](https://www.llamaindex.ai/pricing) |

The Mistral price comes from the OCR 3 announcement.
The Mistral pricing page did not show an OCR price in a form that this research could read.
The Mistral free mode does not include API access ([tiers](https://docs.mistral.ai/deployment/laplateforme/tier/)).
With LlamaParse, 10,000 credits cover 10,000 pages at the Fast tier, or 1,000 pages at the Agentic tier ([pricing](https://www.llamaindex.ai/pricing)).
Whether Google Cloud, Azure, and AWS each need a billing account for the free allowance was Not verified.

### Handwriting and Filipino support

| Option | Handwriting claim | Filipino printed text | Filipino handwriting | Source |
| --- | --- | --- | --- | --- |
| Tesseract | "won't work very well" | `fil` and `tgl` models exist | No claim | [FAQ](https://tesseract-ocr.github.io/tessdoc/FAQ.html), [tessdata](https://github.com/tesseract-ocr/tessdata) |
| Google Cloud Vision | `DOCUMENT_TEXT_DETECTION` detects handwriting | Filipino (`fil`) and Tagalog listed | Not verified | [handwriting](https://cloud.google.com/vision/docs/handwriting), [languages](https://cloud.google.com/vision/docs/languages) |
| Azure Read | Handwriting in 12 languages | Filipino (`fil`) and Tagalog (`tl`) listed for print | Not in the handwriting list | [language support](https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/language-support/ocr) |
| AWS Textract | English handwriting only | More than 40 languages. Filipino Not verified. | No | [FAQ](https://aws.amazon.com/textract/faqs/) |
| Mistral OCR 3 | Vendor claims gains on handwriting and cursive | Not verified | Not verified | [announcement](https://mistral.ai/news/mistral-ocr-3) |
| Claude, Gemini, OpenAI | No handwriting statement on the pages read | Not stated | Not stated | Pages in the direct input section |

No vendor publishes handwriting accuracy figures on these pages.
Handwriting results for Suro can only come from tests on real materials.

## Parsing libraries

### Python

| Library | License | What it reads | Failure cases | Source |
| --- | --- | --- | --- | --- |
| `pypdf` | BSD-3-Clause | PDF text layer | "pypdf is not OCR software." For a scanned PDF, the docs advise you to use OCR directly. | [PyPI](https://pypi.org/project/pypdf/), [extract text](https://pypdf.readthedocs.io/en/stable/user/extract-text.html) |
| `pdfplumber` | MIT | PDF characters, lines, and tables | Works best on machine-generated PDFs, not scanned ones. No OCR. | [README](https://github.com/jsvine/pdfplumber) |
| `pdfminer.six` | MIT | PDF layout and text | Not verified | [repo](https://github.com/pdfminer/pdfminer.six) |
| `pypdfium2` | BSD-3-Clause, Apache-2.0 | PDFium bindings. Capabilities Not verified. | Not verified | [PyPI](https://pypi.org/project/pypdfium2/) |
| PyMuPDF | AGPL-3.0, or a commercial license | PDF, XPS, EPUB, images. OCR through RapidOCR or Tesseract. | PPTX needs `pymupdf-office`, which is commercial. Without a key it reads only the first 3 pages. | [README](https://github.com/pymupdf/PyMuPDF), [pymupdf-office on PyPI](https://pypi.org/project/pymupdf-office/) |
| Docling | MIT | PDF, DOCX, PPTX, XLSX, HTML, images. OCR for scanned PDFs and images. | Legacy DOC, XLS, and PPT need LibreOffice. | [README](https://github.com/docling-project/docling), [formats](https://docling-project.github.io/docling/usage/supported_formats/) |
| Marker | Code Apache-2.0. Model weights modified OpenRAIL-M. | PDF to Markdown | Weights are free for research, personal use, and startups under $5M in funding or revenue. | [README](https://github.com/datalab-to/marker) |
| MinerU | Apache-2.0 with extra terms | PDF parsing | An online service must say that it uses MinerU. A commercial license applies above 100 million "monthly active users (MAU)" or $20M monthly revenue. | [LICENSE](https://github.com/opendatalab/MinerU/blob/master/LICENSE.md) |
| `python-pptx` | MIT | PPTX shapes that have a text frame | The documented text example reads only text frames, so text inside a picture is skipped. Last push 2024-08-07. | [quickstart](https://python-pptx.readthedocs.io/en/latest/user/quickstart.html), [repo](https://github.com/scanny/python-pptx) |
| MarkItDown | MIT | PDF, PowerPoint, Word, Excel, images | Image captioning uses web-hosted services. The `markitdown-ocr` plugin uses an LLM for OCR. | [README](https://github.com/microsoft/markitdown) |
| `unstructured` | Apache-2.0 | Not verified | Not verified | [PyPI](https://pypi.org/project/unstructured/) |
| OCRmyPDF | MPL-2.0 | Not verified | Not verified | [repo](https://github.com/ocrmypdf/OCRmyPDF) |
| LiteParse | Apache-2.0 | Local PDF parsing with bounding boxes. Also on npm. | Its README says scanned PDFs, handwriting, and dense tables do better on LlamaParse. | [README](https://github.com/run-llama/liteparse) |

### TypeScript

| Library | License | What it reads | Failure cases | Source |
| --- | --- | --- | --- | --- |
| `pdfjs-dist` (PDF.js) | Apache-2.0 | PDF rendering and text | No OCR found in the README. Not verified. | [npm](https://www.npmjs.com/package/pdfjs-dist), [repo](https://github.com/mozilla/pdf.js) |
| `unpdf` | MIT | Not verified | Not verified | [npm](https://www.npmjs.com/package/unpdf) |
| `pdf-parse` | Apache-2.0 | PDF text. Repo is now `mehmet-kozan/pdf-parse`. | Not verified | [npm](https://www.npmjs.com/package/pdf-parse) |
| `tesseract.js` | Apache-2.0 | Images, in the browser or Node.js | No PDF support. Same handwriting limits as Tesseract. | [README](https://github.com/naptha/tesseract.js) |
| `mupdf` (mupdf.js) | AGPL-3.0-or-later | PDF and more | AGPL network clause, below | [npm](https://www.npmjs.com/package/mupdf) |
| `officeparser` | MIT | Office files. Capabilities Not verified. | Not verified | [npm](https://www.npmjs.com/package/officeparser) |
| `pptxtojson` | MIT | PPTX. Capabilities Not verified. | Not verified | [npm](https://www.npmjs.com/package/pptxtojson) |

Version numbers and licenses come from PyPI, npm, and the GitHub API on 2026-11-26.

### Why AGPL matters

AGPL-3.0 section 13 covers network use.
If you modify the program, you must offer its source to every person who uses it over a network ([AGPL-3.0 text](https://choosealicense.com/licenses/agpl-3.0/)).
This research read that text through the GitHub licenses API.
Suro is a web app, so this clause applies to PyMuPDF, `mupdf`, and Scribe.js.
This research does not give legal advice.

### Hosting limits that affect libraries

- Vercel Functions accept request bodies up to 4.5 MB by default ([Vercel limits](https://vercel.com/docs/functions/limitations)).
  Many PDFs and phone photos are larger, so a material may need to go straight to storage.
- A Vercel Python function can be up to 500 MB uncompressed, with 2 GB of memory on Hobby ([Vercel limits](https://vercel.com/docs/functions/limitations)).
- Render spins down a free web service after 15 minutes without traffic ([Render free](https://render.com/docs/free)).
- The install size and memory use of Docling, Marker, and other ML-based parsers were Not verified.
  They load ML models, so free plans can be too small for them.

## By material type

| Material | Works reliably | Where it fails |
| --- | --- | --- |
| Text PDF | Any parsing library reads the text layer exactly. All three model APIs read it directly. | The `pypdf` docs say that PDFs have no semantic layer, so tables and captions have no clear extraction rule ([pypdf](https://pypdf.readthedocs.io/en/stable/user/extract-text.html)). Encrypted PDFs fail on Claude. |
| Scanned PDF | Direct model input, or cloud OCR | Text-layer libraries read only a scanner's OCR layer, with its errors ([pypdf](https://pypdf.readthedocs.io/en/stable/user/extract-text.html)). Tesseract fails on skew, low DPI, and noise. |
| Slides (PPTX) | Gemini 4 reads slide images directly. Docling and MarkItDown read PPTX text. | The Claude PDF guide does not list PPTX. OpenAI reads text only. `python-pptx` skips text inside pictures. |
| Phone photo | Gemini accepts HEIC. All three APIs read JPEG. | Claude and OpenAI reject HEIC. Rotated, small, or compressed text lowers accuracy on all three. |
| Handwriting | Cloud Vision and Azure claim handwriting support. Mistral claims gains. | Tesseract is weak. Azure does not list Filipino handwriting. Textract reads only English handwriting. No accuracy figures exist. |

### Phone photos in a phone browser

Safari can convert a HEIC photo to JPEG when the file input `accept` list excludes HEIC.
This behavior appears only in WebKit bug reports, not in Apple documentation ([WebKit bug 212489](https://bugs.webkit.org/show_bug.cgi?id=212489)).
Treat it as partly verified.
Later reports describe Safari 17 and newer converting files to HEIC when `accept` includes `image/heic` ([Apple forum](https://developer.apple.com/forums/thread/743049)).
These are secondary reports.
Android browser behavior was Not verified.

Claude accepts images up to 10 MB and 8000 x 8000 px ([Vision](https://platform.claude.com/docs/en/build-with-claude/vision)).
If you resize and compress a photo in the browser before you send it, you avoid the size limits.
Do not compress too much, because heavy JPEG compression makes text hard to read ([Vision](https://platform.claude.com/docs/en/build-with-claude/vision)).

## What this means for Suro

This section lists trade-offs.
A later ticket with the owner makes the decision.

- **One API for all types.** Gemini 4 covers PDF, PPTX slide images, and HEIC in one API.
  Gemini 4 Hydrogen costs about $0.84 per 1,000 pages, which is more than Gemini Flash-Lite.
  The cheaper Gemini 3.x models read PPTX as text and embedded images.
  Claude needs a PPTX-to-PDF step and a HEIC-to-JPEG step.
- **Free plan or privacy.** The Gemini free plan costs nothing.
  Its terms say not to send personal information, and learners' materials can contain it.
- **Read once, or read every turn.** Extracting text once and storing it costs one read.
  Sending the file on every turn costs a read on every turn, unless caching applies.
- **Text layer first.** For a text PDF, a free MIT or BSD library gives exact text.
  You can then send only page images that need vision, such as charts or scans.
- **Handwriting needs a test set.** No source gives accuracy numbers.
  A small test set of real Filipino and English materials would show which option works.
- **License check.** An AGPL library is a fit if you plan to publish Suro's source.
  If you do not, the permissive libraries avoid the network clause.

## Not verified

- Free-plan request limits for each Gemini model.
- OpenAI PDF page limit, PDF tokens per page, and an OpenAI API free plan.
- Which OpenAI models accept images.
- Whether the Document AI free 1,000 pages reset each month.
- Filipino support in AWS Textract, EasyOCR, PaddleOCR, and Mistral OCR 3.
- Handwriting accuracy for any option.
- The amount of monthly API credit in Claude Max, Ultra, and Team plans.
- iOS and Android browser photo conversion rules, beyond bug reports.
- Capabilities of `pdfminer.six`, `pypdfium2`, `unstructured`, OCRmyPDF, `unpdf`, `pdf-parse`, `officeparser`, and `pptxtojson`.
- Install size and memory use of ML-based parsers on free hosting.

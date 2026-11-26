# Checkable sources for explanations beyond the material

Research for issue #5. All pages were read on 2026-11-26. Prices are in USD and can change after that date.

In this file, "source" always means an outside reference that a learner can check. It never means the material.

A failure case without a link is my own reasoning, not a documented fact.

## Short answer

You can attach a checkable source in three ways:

1. Let the model search the web with a first-party tool (Claude, Gemini, or OpenAI). The tool returns URLs and cited spans.
2. Run your own search (a third-party search API or an open corpus such as Wikipedia). Then pass the results to the model as citable blocks.
3. Pass passages, from the material or from a fixed corpus, to the model with citations turned on.

No option proves that a source supports a claim. The APIs guarantee, at most, that a quoted span exists in the text they were given. In one study, only 51.5% of sentences from generative search engines were fully supported by their citations ([Liu et al. 2023](https://arxiv.org/abs/2304.09848)). So you need your own check after generation:

1. Check that the link loads.
2. Check that the quoted passage is on the page.
3. Check that the passage supports the claim.

## Option 1: first-party grounding tools

### Claude web search tool

- Price: "$10 USD per 1,000 searches, plus standard token costs for search-generated content." Search results count as input tokens in later turns too. A failed search is not billed. ([web search tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool), [pricing](https://platform.claude.com/docs/en/about-claude/pricing), read 2026-11-26)
- Citations: "Citations are always enabled for web search." Each citation has `url`, `title`, `encrypted_index`, and `cited_text`. `cited_text` is "Up to 150 characters of the cited content." ([web search tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool))
- The page text of each result comes back only as `encrypted_content`. You get the URL, title, and `page_age`, but not the full text. ([web search tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool))
- Controls: `max_uses`, `allowed_domains` or `blocked_domains` (not both), and `user_location`. ([web search tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool))
- Display rule: if you show the output directly to people, you must include the citations to the original source. ([web search tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool))
- Data: the basic version `web_search_20250305` is eligible for Zero Data Retention. Versions `_20260209` and later use dynamic filtering and are not eligible by default. ([server tools](https://platform.claude.com/docs/en/agents-and-tools/tool-use/server-tools))

Failure cases:

- Claude may not search. The docs say Claude answers "Established facts, math, science fundamentals, or coding concepts" without searching. Most tutoring explanations fall in this group. ([web search tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool))
- `tool_choice` can force a named tool, but "Not every model and setting supports forced tool use." ([define tools](https://platform.claude.com/docs/en/agents-and-tools/tool-use/define-tools)). Not verified: whether `tool_choice` can force the `web_search` server tool on the model Suro picks.
- Errors return inside a 200 response: `too_many_requests`, `invalid_tool_input`, `max_uses_exceeded`, `query_too_long`, `request_too_large`, and `unavailable`. ([web search tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool))
- An administrator can turn off web search in the Claude Console. Then the request fails with a 400 error. ([web search tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool))
- The 150-character `cited_text` is too short to show that a page supports a longer claim.

### Claude web fetch tool

- Price: "The web fetch tool is available on the Claude API at no additional cost." You pay tokens for the fetched content. The docs estimate about 2,500 tokens for an average 10 kB web page. ([web fetch tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-fetch-tool), read 2026-11-26)
- Citations are optional and "disabled by default." Set `"citations": {"enabled": true}`. ([web fetch tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-fetch-tool))
- The fetched text comes back in the response, so your server can read it. ([web fetch tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-fetch-tool))
- Claude "can only fetch URLs that have previously appeared in the conversation context." This includes URLs from earlier search results. ([web fetch tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-fetch-tool))

Failure cases:

- It "does not support websites dynamically rendered with JavaScript." ([web fetch tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-fetch-tool))
- Error codes include `url_not_accessible` (HTTP error), `url_not_allowed` (including `robots.txt`), `url_not_in_prior_context`, `unsupported_content_type` (only text, HTML, and PDF), and `content_too_large`. ([web fetch tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-fetch-tool))
- The docs warn of "data exfiltration risks" when Claude processes untrusted input next to sensitive data. A material is untrusted input. ([web fetch tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-fetch-tool))
- Large pages cost many tokens. Use `max_content_tokens` to cap them. ([web fetch tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-fetch-tool))

### Claude citations feature and search result blocks

This mechanism serves both the material and outside sources.

- Citations work on plain text, PDF, and custom content blocks. Plain text gives character indices. PDF gives page numbers. Custom content gives block indices. ([citations](https://platform.claude.com/docs/en/build-with-claude/citations))
- Cost: "a slight increase in input tokens." The `cited_text` field "does not count toward output tokens." ([citations](https://platform.claude.com/docs/en/build-with-claude/citations))
- Guarantee: the docs say citations always hold valid pointers into the content you sent. This means the quoted span exists in the text you sent. It does not mean that the span supports the claim. ([citations](https://platform.claude.com/docs/en/build-with-claude/citations))
- `search_result` blocks let you pass your own retrieved passages with a `source` URL and `title`. Claude then cites them as `search_result_location`. The `cited_text` is the full text of the cited blocks. ([search results](https://platform.claude.com/docs/en/build-with-claude/search-results))
- You can return `search_result` blocks from your own tool. This is how you run your own search (option 2) and keep Claude's citation format. ([search results](https://platform.claude.com/docs/en/build-with-claude/search-results))

Failure cases:

- Claude cannot cite images. So it cannot cite a scanned PDF that has no extractable text. A scanned material cannot be cited without OCR first. ([citations](https://platform.claude.com/docs/en/build-with-claude/citations))
- Citations and structured outputs are incompatible. A request that uses both returns a 400 error. ([citations](https://platform.claude.com/docs/en/build-with-claude/citations))
- One request cannot mix citation-enabled and citation-disabled content. ([citations](https://platform.claude.com/docs/en/build-with-claude/citations), [search results](https://platform.claude.com/docs/en/build-with-claude/search-results))
- If the prompt asks for only JSON, "the response might contain few or no citations." ([search results](https://platform.claude.com/docs/en/build-with-claude/search-results))

### Gemini grounding with Google Search

- Price on Gemini 3 and newer models: billing counts "each search query that the model decides to execute." The first 5,000 searches each month are free, shared across those models. After that, the price is "$14 per 1,000 search requests." ([google-search](https://aistudio.google.com/docs/google-search.md), [pricing](https://aistudio.google.com/docs/pricing.md), read 2026-11-26)
- A daily cap of 1,500 requests per day applies on the paid tier. ([pricing](https://aistudio.google.com/docs/pricing.md))
- Free tier: for Gemini 3 and newer models, grounding is "Not available" on the free tier and "Can be tested in Google AI Studio." ([pricing](https://aistudio.google.com/docs/pricing.md))
- Gemini 2.5 Flash-Lite still lists free grounding "up to 500 RPD." The paid tier gets 1,500 RPD free, then "$35 / 1,000 grounded prompts." ([pricing](https://aistudio.google.com/docs/pricing.md)). `gemini-2.5-flash-lite` has "No shutdown date announced." `gemini-2.5-flash` shuts down on December 1, 2026. ([deprecations](https://aistudio.google.com/docs/deprecations.md))
- On the free tier, "Used to improve our products" is "Yes." ([pricing](https://aistudio.google.com/docs/pricing.md))
- Response: `url_citation` annotations with `url`, `title`, `start_index`, and `end_index` on the answer text. ([google-search](https://aistudio.google.com/docs/google-search.md))
- Billed queries "can be higher than the queries listed" in the response. ([google-search](https://aistudio.google.com/docs/google-search.md))

Failure cases and terms:

- You must show grounded results together with their Search Suggestions. You may show them only to the person who sent the prompt. ([terms](https://aistudio.google.com/docs/terms.md))
- Google stores prompts and output "for thirty (30) days" for grounding. ([terms](https://aistudio.google.com/docs/terms.md))
- The API terms set a minimum age of 18. They also bar apps that are likely to reach people under 18. Suro already limits learners to adults. ([terms](https://aistudio.google.com/docs/terms.md))
- The response gives no quoted passage from the page. You get only the span of the answer text and a URL.
- Not verified: whether the returned URLs are direct page URLs or Google redirect URLs. The Interactions API example shows direct URLs. A redirect URL would change how you run a link check.

### OpenAI web search

- Price: "Web search (incl. images) | $10.00 / 1k calls + Search content tokens billed at model rates." The preview tool on non-reasoning models costs $25.00 per 1,000 calls. ([pricing](https://developers.openai.com/api/docs/pricing), read 2026-11-26). I found no free allowance on that page.
- Citations: `url_citation` annotations with URL, title, and start and end index. A `sources` field lists every URL the model consulted. ([web search](https://developers.openai.com/api/docs/guides/tools-web-search))
- Display rule: "you must show inline citations as prominent, clickable links." ([web search](https://developers.openai.com/api/docs/guides/tools-web-search))
- Controls: up to 100 `allowed_domains` or `blocked_domains`, `user_location`, and `external_web_access: false` for cache-only results. ([web search](https://developers.openai.com/api/docs/guides/tools-web-search))

Failure cases:

- "With `tool_choice: "auto"`, search is optional." Use `tool_choice: "required"` to make search run. ([web search](https://developers.openai.com/api/docs/guides/tools-web-search))
- The web search context window is limited to 128k tokens. ([web search](https://developers.openai.com/api/docs/guides/tools-web-search))
- Like Gemini, the annotation marks a span of the answer, not a quoted passage from the page.

## Option 2: third-party search APIs

With these, your server runs the search. Then you pass the results to the model as `search_result` blocks or plain text. You choose when a search runs, so the model cannot skip it.

| Service | Free plan | Paid price | Notes |
| --- | --- | --- | --- |
| Brave Search API | "$5 in free credits every month" | Search: "$5 per 1,000 requests" | Storing results needs a custom plan |
| Tavily | 1,000 credits per month | Pay as you go: $0.008 per credit | Basic search: 1 credit. Advanced: 2 credits |
| Exa | "$10 credits a month" (about 2,500 Lite searches) | Lite $4, Auto $7 per 1,000 requests | Page contents: $1 per 1,000 pages |
| Serper (Google results) | "2,500 free queries", no credit card | $1.00 per 1,000 at the 50,000-credit pack | Credits valid for 6 months |
| SerpApi | 250 searches per month | Starter: $25 for 1,000 searches | Only successful searches count |
| Google Custom Search JSON API | Closed to new customers | Not available | Ends January 1, 2027 |
| Bing Search APIs | Retired | Not available | Retired August 11, 2025 |

Sources, all read 2026-11-26: [Brave](https://brave.com/search/api/), [Tavily](https://docs.tavily.com/documentation/api-credits), [Exa](https://exa.ai/pricing), [Serper](https://serper.dev/), [SerpApi](https://serpapi.com/pricing), [Google Custom Search](https://developers.google.com/custom-search/v1/overview), and [Bing retirement](https://learn.microsoft.com/en-us/lifecycle/announcements/bing-search-api-retirement).

Not verified:

- The Brave page also says "Up to $20 in free calls every month." I could not confirm how this relates to the $5 credit on each plan.
- I could not confirm whether the Serper 2,500 free queries are one-time or monthly.

Failure cases:

- You build and maintain the search tool, the retries, and the key storage.
- Snippets can be short or stale. You may need a second fetch to get the passage.
- Storage terms vary. Brave says storing results needs a custom plan. A cache of sources can break a term. ([Brave](https://brave.com/search/api/))
- Free plans are small. One learner session can use many searches.
- Brave publishes its own benchmark against Exa and Tavily. That is a vendor claim, not an independent result. ([Brave](https://brave.com/search/api/))

## Option 3: fixed open corpora

A fixed corpus is cheaper and narrower. It also gives stable pages that are easier to check.

### Wikipedia (MediaWiki APIs)

- Price: free. Search uses `action=query&list=search`. ([API:Search](https://www.mediawiki.org/wiki/API:Search))
- Rate limits are new in 2026 and "subject to experimentation and change." Requests with a compliant `User-Agent` get 200 requests per minute. Unidentified requests get 10 per minute. ([rate limits](https://www.mediawiki.org/wiki/Wikimedia_APIs/Rate_limits), read 2026-11-26)
- Unauthenticated requests from a web browser also get 200 requests per minute. ([rate limits](https://www.mediawiki.org/wiki/Wikimedia_APIs/Rate_limits))
- A `User-Agent` header is required. A client that sends no `User-Agent` header can be blocked without notice. ([access policy](https://www.mediawiki.org/wiki/Wikimedia_APIs/Access_policy))
- License: most text is under CC BY-SA 4.0. ([Wikipedia:Copyrights](https://en.wikipedia.org/wiki/Wikipedia:Copyrights))
- Each revision has a permanent `oldid` URL. You can cite the exact version that Suro read. ([example oldid link](https://www.mediawiki.org/w/index.php?title=Wikimedia_APIs/Rate_limits&oldid=8703424))

Failure cases:

- Coverage is broad but shallow for some subjects. Some articles change often or contain errors.
- A 429 status means you hit the limit. Follow the `Retry-After` header. ([rate limits](https://www.mediawiki.org/wiki/Wikimedia_APIs/Rate_limits))
- Some learners and teachers do not accept Wikipedia as a source.

### OpenStax

- I found no documented public API for OpenStax content.
- Observed on 2026-11-26: an undocumented content-system endpoint lists 129 book entries with license fields. ([CMS endpoint](https://openstax.org/apps/cms/api/v2/pages/?type=books.Book&fields=title,license_name,book_state&limit=200))
- Observed on 2026-11-26: `https://openstax.org/rex/release.json` points to a JSON archive of book contents. A fetch of one book returned its title and table of contents.
- Licenses observed in that endpoint: of 84 live entries, 70 list CC BY-NC-SA 4.0, 4 list CC BY 4.0, and 10 have no license field. For example, Biology 2e lists CC BY-NC-SA 4.0.
- The "NonCommercial" term matters if Suro becomes a product. I did not check the license text for linking or short quotes. This is not legal advice.

Failure cases:

- The endpoints are undocumented, so they can change without notice.
- There is no search endpoint. You must build your own index.
- Coverage is limited to the 84 live book entries, mostly introductory college and some school subjects (for example, Algebra 1).

### Crossref, for scholarly sources

- Use Crossref to confirm that a DOI exists and to read its metadata. "No sign-up is required to use the REST API." Add `mailto` to use the polite pool. ([Crossref REST API](https://www.crossref.org/documentation/retrieve-metadata/rest-api/))
- This checks existence only. It does not check that the paper supports a claim.

## Citing passages inside the material

You can use the same mechanism for the material and for outside sources:

- Pass the material as a Claude `document` with citations turned on. You get page numbers for a PDF or character indices for text. ([citations](https://platform.claude.com/docs/en/build-with-claude/citations))
- Pass material chunks as custom content or `search_result` blocks. Each block becomes the smallest citable unit. ([citations](https://platform.claude.com/docs/en/build-with-claude/citations), [search results](https://platform.claude.com/docs/en/build-with-claude/search-results))
- If you pass outside passages as `search_result` blocks too, both kinds of citation have the same response shape. Your app then needs one renderer and one checker.
- Gemini File Search returns `file_citation` annotations. Storage and query-time embeddings are free. Indexing embeddings and retrieved tokens are billed. ([file search](https://aistudio.google.com/docs/file-search.md))
- OpenAI file search costs "$2.50 / 1k calls" plus storage at "$0.10 / GB per day," with 1 GB free. ([pricing](https://developers.openai.com/api/docs/pricing))

Failure case: a scanned material has no text to cite until you run OCR.

## How to confirm that a source exists and supports the claim

These are three separate checks. Each one catches a different failure.

### Check 1: the source exists

1. Send a request to the URL from your server.
2. Accept the source only if the status is 200 and the body is not empty. SourceCheckup uses this rule for URL validity. ([Wu et al. 2025](https://www.nature.com/articles/s41467-025-58551-6))
3. If the check fails, query the Wayback Machine availability API: `http://archive.org/wayback/available?url=...`. It returns the closest snapshot or an empty object. ([Wayback API](https://archive.org/help/wayback_api.php))
4. For a DOI, query Crossref. ([Crossref REST API](https://www.crossref.org/documentation/retrieve-metadata/rest-api/))

Failure cases: some pages block bots, need JavaScript, or are behind a paywall. A page can return 200 with a soft "not found" message.

### Check 2: the quoted passage is on the page

1. Fetch the page text. Use Claude `web_fetch` or your own fetch.
2. Normalize case and white space in the page and the quote.
3. Check that the quote is a substring of the page.
4. For Claude web search, remove the trailing "..." from `cited_text` before you match. The field is cut at 150 characters. ([web search tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool))

Note: Claude citations on content you sent pass this check by design. Gemini and OpenAI web search do not return a page quote, so you must ask the model for one or extract one yourself.

Failure cases: the page changed since the search, dynamic pages differ per visit, and PDF text extraction can break words.

### Check 3: the passage supports the claim

1. Send the claim and the passage to a second model call.
2. Ask for one label: supported, not supported, or contradicted.
3. Show the source only if the label is "supported."

Evidence that this works: in SourceCheckup, an LLM judge found 42.4% of GPT-4o RAG responses fully supported. A human expert found 40.4% on the same questions. ([Wu et al. 2025](https://www.nature.com/articles/s41467-025-58551-6))

Failure cases: the judge model can be wrong. Each check adds one model call of cost and delay.

### Show the passage to the learner

- Link with a text fragment: `https://example.com#:~:text=start,end`. Supporting browsers scroll to and highlight the passage. ([MDN text fragments](https://developer.mozilla.org/en-US/docs/Web/URI/Reference/Fragment/Text_fragments))
- Support: Chrome 80, Safari 16.1, and Firefox 131 and later. This comes from the `html.elements.a.text_fragments` entry in MDN browser-compat-data 8.1.11 ([data file](https://unpkg.com/@mdn/browser-compat-data/data.json), read 2026-11-26).
- If the text does not match, the browser opens the top of the page. So the link still works. ([MDN text fragments](https://developer.mozilla.org/en-US/docs/Web/URI/Reference/Fragment/Text_fragments))
- Also show the quoted passage and the date Suro read it. The learner can then see what Suro relied on, even if the page changes later.

## What research says about citation accuracy

All of these studies test consumer products or models from 2023 to 2025. None tests the API citation features above. Use them as a baseline risk, not as a measure of those features.

- Generative search engines: "a mere 51.5% of generated sentences are fully supported by citations and only 74.5% of citations support their associated sentence." ([Liu et al. 2023](https://arxiv.org/abs/2304.09848))
- ALCE benchmark: "on the ELI5 dataset, even the best models lack complete citation support 50% of the time." ([Gao et al. 2023](https://arxiv.org/abs/2305.14627))
- Fabricated references without search: "55% of the GPT-3.5 citations but just 18% of the GPT-4 citations are fabricated." ([Walters and Wilder 2023](https://www.nature.com/articles/s41598-023-41032-5))
- Medical questions, seven models, several without web access: "between 50% and 90% of LLM responses are not fully supported, and sometimes contradicted, by the sources they cite." ([Wu et al. 2025](https://www.nature.com/articles/s41467-025-58551-6))
- Same study: "Even for GPT-4o with Web Search, approximately 30% of individual statements are unsupported." ([Wu et al. 2025](https://www.nature.com/articles/s41467-025-58551-6))
- AI search engines on news: they "provided incorrect answers to more than 60 percent of queries." Grok 3 had 154 citations to error pages in 200 prompts. ([Tow Center, March 2025](https://www.cjr.org/tow_center/we-compared-eight-ai-search-engines-theyre-all-bad-at-citing-news.php))
- EBU and BBC study, October 2025: "45% of all AI answers had at least one significant issue, and 31% of responses showed serious sourcing problems." This figure comes from NPR, a study participant. I could not open the EBU page (HTTP 403). ([NPR](https://www.npr.org/sections/npr-extra/2025/10/21/g-s1-94424/global-study-on-news-integrity-in-ai-assistants-shows-need-for-safeguards-and-improved-accuracy))

My inference from these studies: search access does not stop unsupported claims. It does not reliably stop broken or fabricated links either. Over half of Gemini and Grok 3 responses cited fabricated or broken URLs ([Tow Center](https://www.cjr.org/tow_center/we-compared-eight-ai-search-engines-theyre-all-bad-at-citing-news.php)). So you need both a link check and a claim-support check.

## Cost summary

| Option | Price per search or request (read 2026-11-26) | Free allowance |
| --- | --- | --- |
| Claude web search | $10 per 1,000 searches plus tokens | None found |
| Claude web fetch | Tokens only | Not applicable |
| Claude citations and `search_result` blocks | Small input-token increase | Not applicable |
| Gemini 3+ grounding | $14 per 1,000 search queries plus tokens | 5,000 per month on paid tier |
| Gemini 2.5 Flash-Lite grounding | $35 per 1,000 grounded prompts | 500 per day on free tier |
| OpenAI web search | $10 per 1,000 calls plus tokens | None found |
| Brave Search API | $5 per 1,000 requests | $5 credit per month |
| Tavily | $0.008 per credit | 1,000 credits per month |
| Exa Lite search | $4 per 1,000 requests | $10 credit per month |
| Wikipedia APIs | Free | 200 requests per minute |

For token cost on Claude, the pricing page lists Sonnet 5.5 at $2 input and $10 output per million tokens. Haiku 5.5 is $0.10 input and $0.50 output for prompts up to 100,000 tokens. ([pricing](https://platform.claude.com/docs/en/about-claude/pricing))

Example arithmetic, not a measurement: one Claude search with 10,000 tokens of results on Sonnet 5.5 costs about $0.01 plus $0.02.

## What this means for Suro

These are trade-offs for the decision ticket, not a decision.

- Claude may skip search for "established facts." Those are most tutoring explanations. If Suro needs a source every time, your server must run the retrieval, or you must force a tool.
- App-side retrieval plus `search_result` blocks gives one citation format for the material and for outside sources. It also lets you choose a free or cheap search back end.
- First-party web search is the least code. Each one has display rules that the Suro interface must follow.
- Gemini's free grounding works only on an older model. On the free tier, Google uses the data to improve its products. Learners may not expect that.
- A fixed corpus such as Wikipedia is free and stable, but it cannot cover every material.
- Whichever option you pick, the three checks above (exists, quote present, supports) decide whether a learner can trust the source.
- Not verified here: whether Claude Max credits cover API tool fees. Another ticket covers AI usage.

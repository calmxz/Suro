# AI access and budget for Suro

Research for issue #2: "What AI access and budget does Suro actually have?"

All pages in this file were read on 2026-11-26. Prices and terms change often. Check each page again before you rely on a number.

## Short answer

- You cannot sign in to a Claude Max plan from Suro and serve learners with it. Anthropic does not permit that.
- A Claude Max plan now includes $100 of Claude API credits each month. You claim them into a Claude Console organization and use them with an API key.
- That API key path is the normal way to build an app on Claude. In my reading of the terms, it can serve learners. No page says so in exact words.
- One 20-turn tutoring session on a 20-page material costs about $0.30 on Claude Sonnet 5.5 with prompt caching. Without caching, it costs about $1.95.
- At that rate, $100 of monthly credits covers about 330 sessions on Sonnet 5.5. Claude Haiku 5.5 is about 15 times cheaper, but its tutoring quality is untested.
- Bring-your-own-key works through a pasted API key or through Anthropic "linked billing". Both ask each learner to make a Claude Console account and buy credits.
- Free plans exist at Google (Gemini API) and OpenRouter. Each has limits, and the Gemini free tier lets Google use your prompts to improve its products.

## Can a Claude Max plan power a deployed app?

There are three separate ways a Max plan could relate to Suro. Each one has a different answer.

### Route 1: sign in with the Max plan and serve learners. Not permitted.

The Claude Code "Legal and compliance" page has a section named "Authentication and credential use". It says:

> "Anthropic does not permit third-party developers to offer Claude.ai login into their own applications, or to route requests through Free, Pro, Max, or Ultra plan credentials on behalf of their users."

The same section also says:

> "Developers building products or services that interact with Claude's capabilities, including those using the [Agent SDK], should use API key authentication through Claude Console or a supported cloud provider."

It adds that plan limits assume one person:

> "Advertised usage limits for Pro, Max, and Ultra plans assume ordinary, individual usage of Claude Code and the Agent SDK."

Source: <https://code.claude.com/docs/en/legal-and-compliance>

The Agent SDK overview says the same thing:

> "Unless previously approved, Anthropic does not allow third party developers to offer claude.ai login or rate limits for their products, including agents built on the Claude Agent SDK. Use the API key authentication methods described in the Quickstart instead."

Source: <https://code.claude.com/docs/en/agent-sdk/overview>

The Consumer Terms of Service (effective Oct 8, 2025) govern the Max plan. Section 2 says:

> "You may not share your Account login information, Anthropic API key, or Account credentials with anyone else. You also may not make your Account available to anyone else."

Section 3 forbids automated access, with one exception:

> "Except when you are accessing our Services via an Anthropic API Key or where we otherwise explicitly permit it, to access the Services through automated or non-human means, whether through a bot, script, or otherwise."

Source: <https://www.anthropic.com/legal/consumer-terms>

A server that runs Suro for strangers on a Max plan login breaks both rules. It makes the account available to other people, and it accesses the plan through a script.

### Route 2: the monthly API credits that come with Max. Usable with an API key.

The pricing page lists "$100 a month in API credits to build on the Claude Platform" for Max ($100 per month). It lists $200 a month for Ultra ($200 per month). Source: <https://claude.com/pricing>

The support article on these credits says:

> "Max, Ultra, and Team plans include monthly credits for the Claude Platform. Use them to build Claude into your own apps and agents with the Claude API."

Source: <https://support.claude.com/en/articles/17154008-monthly-api-credits-for-max-ultra-and-team-plans>

Key facts from that article:

- You claim the credits on claude.ai, under Settings, then Billing, then "Link organization".
- You can link only one Claude Console organization. You cannot change it yourself.
- You must be on the plan for seven days before you can claim.
- The credits cover the Messages API, the Message Batches API, the Console Playground, Claude Managed Agents, and the Agent SDK.
- The credits do not cover interactive Claude Code, extra usage, or Claude on Bedrock, Vertex AI, or Foundry.
- The credits do not roll over. Unused credits expire at the end of each billing cycle.
- If the organization has no other credits, "API requests stop until your next monthly credits arrive. Usage is never charged to your Claude plan."
- Everyone with an API key in the linked organization draws from the same balance.

Another support article confirms the split. The credits apply "when you run them with an API key from your linked Claude Console organization". When you sign in with the plan instead, usage "still draw[s] from your subscription limits". Source: <https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan>

API key use falls under the Commercial Terms of Service. Section A.1 says:

> "Anthropic gives Customer permission to use the Services, including to power products and services Customer makes available to its own customers and end users ("Users")."

Source: <https://www.anthropic.com/legal/commercial-terms>

The Supplemental Credit Terms apply when you claim. Section B.2 says credits "may only be used by the holder of the Anthropic account to which the Credits are associated." Source: <https://www.anthropic.com/legal/credit-terms>

My reading: Suro's server holds the API key, and the owner's organization holds the credits. Learners never touch the key or the account. That matches Commercial Terms A.1 and Credit Terms B.2. No page I read says in exact words that Max monthly credits may serve the public. Treat this as an interpretation, not a quoted fact. To get certainty, you can ask Anthropic support.

### Route 3: linked billing. Cannot use Max credits.

Linked billing lets a learner pay for their own Claude usage inside Suro. It is covered in the bring-your-own-key section below. It cannot draw on plan credits:

> "Claude subscription plans (Pro, Max, Ultra, Team, and Enterprise) can't be linked"

The list of credits that cannot pay for linked usage includes "Monthly API credits that come with Max, Ultra, and Team plans". Source: <https://support.claude.com/en/articles/18467110-offer-linked-billing-in-your-platform>

### Other Anthropic rules that apply to Suro

- The Usage Policy (effective Nov 12, 2026) requires a chatbot to tell people they talk to AI: "All consumer-facing chatbots or AI agents that communicate directly with external users must disclose to users that they are interacting with AI rather than a human." Source: <https://www.anthropic.com/legal/aup>
- The Usage Policy lists "Education and credentials" as a high-risk area. It covers "grading an exam or assessment that determines admission, graduation, licensure, or certification". Suro's mastery checks do not grant credentials, so this area does not seem to apply. Source: <https://www.anthropic.com/legal/aup>
- The Philippines is on Anthropic's list of supported countries and regions. Source: <https://www.anthropic.com/supported-countries>
- New API organizations start in the Start tier. That tier has a monthly spend cap of $500. Source: <https://platform.claude.com/docs/en/api/rate-limits>
- You can set your own lower monthly spend limit in the Console. When usage reaches it, requests return HTTP 400. Source: <https://platform.claude.com/docs/en/api/rate-limits>

### Owner must confirm

These facts are about the owner's own account. This research does not guess them.

- Which plan the owner has: Max ($100), Ultra ($200), or another plan.
- Whether the owner has been on that plan for at least seven days.
- Whether the "API credits" section appears under Settings, then Billing, on claude.ai.
- Which Claude Console organization to link. The link is permanent unless support changes it.
- Whether the owner wants auto-reload on or off. With auto-reload off and no purchased credits, requests stop when the monthly credits run out. That works as a hard spending cap.
- Whether the owner's Console organization already has other credits or a payment method. Purchased credits are spent after the monthly credits.

## API pricing for models that fit a tutor

Prices are in USD per million tokens (MTok). Source: <https://platform.claude.com/docs/en/about-claude/pricing>, read 2026-11-26.

| Model | Input | 5-minute cache write | 1-hour cache write | Cache read | Output |
| --- | --- | --- | --- | --- | --- |
| Claude Opus 5.5 | $4 | $5 | $8 | $0.20 | $20 |
| Claude Sonnet 5.5 | $2 | $2.50 | $4 | $0.10 | $10 |
| Claude Haiku 5.5, prompt up to 100,000 tokens | $0.10 | $0.125 | $0.20 | $0.01 | $0.50 |
| Claude Haiku 5.5, prompt over 100,000 tokens | $0.50 | $0.625 | $1 | $0.05 | $2.50 |
| Claude Haiku 4.5 | $1 | $1.25 | $2 | $0.10 | $5 |

Notes from the same page:

- The page describes Sonnet 5.5 as "The best combination of speed and intelligence".
- The page describes Haiku 5.5 as "For high-volume, latency-sensitive tasks such as classification, extraction, and routing". Tutoring is not in that list.
- Haiku 5.5 bills a whole request at the higher price when its prompt is over 100,000 tokens. Cached tokens count toward that length.
- The Batch API gives 50 percent off input and output. A live tutoring session cannot use it, but a one-time material analysis can.
- Claude 4.7 and later models use a newer tokenizer. The page says it "produces approximately 30% more tokens for the same text."
- Web search costs $10 per 1,000 searches, plus the search results as input tokens. Web fetch has no extra charge beyond tokens.
- US-only inference costs 1.1 times the standard price. The default global routing uses standard prices.

## Bring your own key

Bring-your-own-key (BYOK) means that each learner pays the AI provider for their own usage. Suro then pays nothing for that learner's tokens.

### Option A: the learner pastes an Anthropic API key

What the learner must do:

1. Create a Claude Console account at <https://platform.claude.com/>.
2. Buy prepaid credits on the Billing page. The API stops when credits run out.
3. Create an API key on the API keys page.
4. Paste the key into Suro.

Sources: <https://platform.claude.com/docs/en/api/overview>, <https://support.claude.com/en/articles/8977456-how-do-i-pay-for-my-claude-api-usage>, and <https://platform.claude.com/docs/en/manage-claude/authentication>.

What it asks of Suro:

- Anthropic says to call the API from your back end: "Anyone who uses a webpage can read an API key in that page's code."
- A browser call needs the `anthropic-dangerous-direct-browser-access: true` header. The TypeScript SDK sends it when you set `dangerouslyAllowBrowser` to `true`.
- If the back end stores learner keys, it must protect them like passwords.
- Purchased credits are non-refundable and expire one year after purchase.

Source for the first two points: <https://platform.claude.com/docs/en/manage-claude/authentication>. Source for the last point: <https://support.claude.com/en/articles/8977456-how-do-i-pay-for-my-claude-api-usage>.

### Option B: Anthropic linked billing (beta)

Linked billing replaces the pasted key. The learner links their Claude Console organization to Suro through OAuth. Suro stores a connection ID, not a key. Anthropic bills the learner directly.

Facts from the developer article (<https://support.claude.com/en/articles/18467110-offer-linked-billing-in-your-platform>):

- Linked billing is in beta. It covers `/v1/messages` and `/v1/chat/completions`.
- You choose on each request who pays: the learner or you.
- Requests run in your organization. All linked learners share your organization's rate limits.
- A new app starts in testing. Only up to 10 listed test accounts can link, each with a monthly limit of $50 or less.
- Before the app goes live, you verify your domain and Anthropic reviews the app.
- If a learner cannot pay, the API returns 402 or 403. You can pause the feature or pay for the request yourself.

What the learner must do (<https://support.claude.com/en/articles/18467148-use-your-claude-platform-account-in-a-third-party-app>):

1. Click the link button in Suro.
2. Sign in to Claude, or create a Claude account and a Console organization.
3. Add credits to the Console organization.
4. Set a monthly spend limit for Suro, or continue without one.
5. Approve the link. Only an Admin or Billing role can approve.

The learner can unlink at any time on the "Linked platforms" page in the Console.

### Option C: OpenRouter OAuth

OpenRouter gives a similar flow across many providers. The learner logs in to OpenRouter and authorizes Suro. Suro exchanges a code for "a user-controlled API key". The flow requires PKCE. Suro can prefill a credit limit and an expiry for the key. OpenRouter says to store the key "encrypted in a server-side database" and never in browser storage. Source: <https://openrouter.ai/docs/guides/overview/auth/oauth>

### What BYOK asks of a learner, in short

- A new account with an AI provider.
- A payment card, and a prepaid purchase before the first session.
- Some technical steps: creating a key, or approving an OAuth link.
- Trust that Suro handles the key or link safely.

For a volunteer from a Facebook group, these steps are a real barrier. The owner must judge how many testers will complete them. Not verified: whether Philippine learners can pay Anthropic or OpenRouter with local cards.

## Other providers and free plans

| Provider | Free plan | Paid price per MTok (input, cached input, output) | Source |
| --- | --- | --- | --- |
| Google Gemini 3.8 Flash | Yes, free of charge on the free tier | $0.75, $0.075, $3.75 until 2026-12-31. Then $1.50, $0.15, $7.50. | Gemini API pricing page |
| Google Gemini 3.1 Flash-Lite | Yes, free of charge on the free tier | $0.25, $0.025, $1.50 | Gemini API pricing page |
| OpenAI GPT-6.1 Terra | Not verified | $1, $0.05, $5 (see note) | <https://developers.openai.com/api/docs/pricing> |
| OpenAI GPT-6 Luna | Not verified | $0.10, $0.01, $0.50 (see note) | <https://developers.openai.com/api/docs/pricing> |
| OpenAI GPT-6.1 Sol | Not verified | $2, $0.10, $10 | <https://developers.openai.com/api/docs/pricing> |
| OpenRouter free models | Yes, models ending in `:free` | $0 for free models | <https://openrouter.ai/docs/faq> |
| Groq | A base plan below the paid Developer plan exists | Not verified | <https://console.groq.com/docs/rate-limits> |

I read the Gemini pricing through Google's official Gemini API docs index, in `gemini-api-guides/pricing.md`. The public page is <https://ai.google.dev/gemini-api/docs/pricing>.

### Google Gemini API

- The free tier is "Unpaid Services". Google uses that content "to provide, improve, and develop Google products". Human reviewers "may read, annotate, and process your API input and output." Source: Gemini API Additional Terms of Service, <https://ai.google.dev/gemini-api/docs/terms>.
- The terms say: "Do not submit sensitive, confidential, or personal information to the Unpaid Services." A learner's material can contain personal information.
- The paid tier is marked "Used to improve our products: No".
- Gemini 2.5 and newer models have implicit caching. Explicit caching also bills storage per hour. Source: `gemini-api-guides/optimization.md` in the same docs index.
- Grounding with Google Search is not available on the free tier for these two models. The paid tier gives 5,000 free searches a month, then $14 per 1,000.
- Not verified: the free-tier rate limits. Google shows them only inside Google AI Studio for each account.
- Not verified: whether the Gemini API free tier is available in the Philippines.

### OpenAI

- The rendered pricing table shows only GPT-6.2 Astra, GPT-6.2 Sol, GPT-6 Astra, and GPT-6.1 Sol.
- Note: I read the Terra and Luna prices from data embedded in the page, not from the visible table. Their column order matches the visible GPT-6.1 Sol row. A second data block lists each price at half. It is probably the Batch or Flex tab. Not verified.
- The page says GPT-5.6 Sol has promotional pricing until November 30, 2026.
- Not verified: whether OpenAI has a free API tier today.

### OpenRouter

From the page code of <https://openrouter.ai/docs/api_reference/limits>:

- Free models allow 20 requests per minute.
- Free models allow 50 requests per day if you bought less than 10 credits.
- Free models allow 1,000 requests per day if you bought at least 10 credits.

The FAQ says free models "have low rate limits" and "are usually not suitable for production use". OpenRouter adds no markup on inference prices, but it charges a fee when you buy credits. Source: <https://openrouter.ai/docs/faq>

### Groq

The rate limits page shows only the Developer plan limits, and it asks you to "Upgrade to Developer plan" for higher limits. The free plan limits did not load. Not verified.

## Cost of one tutoring session

### Assumptions

| Item | Value | Basis |
| --- | --- | --- |
| Material size | 40,000 tokens | A 20-page PDF at 2,000 tokens per page. Anthropic says a PDF page typically uses 1,500 to 3,000 text tokens. |
| Material format | Text only | Suro extracts the text. A PDF sent as images adds up to 1,568 tokens per page. |
| System prompt | 2,000 tokens | Assumption: tutor instructions, mastery rules, and output format. |
| Turns | 20 | Assumption: one turn is one learner message and one tutor reply. |
| Learner message | 100 tokens | Assumption. |
| Tutor reply | 400 tokens | Assumption. No extended thinking tokens are counted. |
| Context | Full history | Each request resends the system prompt, the material, and all earlier turns. |
| Caching | 5-minute cache | Each turn reads the cached prefix and writes only the new 500 tokens. Every turn arrives within 5 minutes. |
| Web search | None | Shown as an add-on below. |
| Main model | Claude Sonnet 5.5 | Prices from the Anthropic table above. |

Source for the page figures: <https://platform.claude.com/docs/en/build-with-claude/pdf-support>

### Token counts

- Input on turn `n` is 2,000 + 40,000 + 500 × (`n` − 1) + 100 tokens.
- Total input over 20 turns is 20 × 42,100 + 500 × (0 + 1 + ... + 19). That is 842,000 + 95,000 = 937,000 tokens.
- Total output is 20 × 400 = 8,000 tokens.
- With caching, turn 1 writes 42,100 tokens. Turns 2 to 20 each write 500 tokens. Total cache writes are 42,100 + 19 × 500 = 51,600 tokens.
- Cache reads are the rest: 937,000 − 51,600 = 885,400 tokens.

### Arithmetic for Claude Sonnet 5.5

Without caching:

- Input: 937,000 × $2 / 1,000,000 = $1.874
- Output: 8,000 × $10 / 1,000,000 = $0.080
- Total: $1.954

With the 5-minute cache:

- Cache writes: 51,600 × $2.50 / 1,000,000 = $0.129
- Cache reads: 885,400 × $0.10 / 1,000,000 = $0.0885
- Output: 8,000 × $10 / 1,000,000 = $0.080
- Total: $0.2975, about $0.30

With the 1-hour cache, writes cost $4 per MTok: 51,600 × $4 / 1,000,000 = $0.206. The total is about $0.375.

A cache miss happens when a learner waits more than 5 minutes between turns. Then the next turn writes the whole prefix again. At turn 10, that prefix is 46,600 tokens. The miss adds 46,600 × ($2.50 − $0.10) / 1,000,000 = about $0.11.

### One-time material analysis

Suro may read the material once to list its concepts. Assume 42,000 input tokens and 2,000 output tokens on Sonnet 5.5. That is 42,000 × $2 / 1,000,000 + 2,000 × $10 / 1,000,000 = $0.104. With the Batch API, it is half: $0.052.

### Other models, same assumptions

Gemini and OpenAI "cached" figures assume the same cache pattern as Claude. They use the provider's cached-input price for reads and the base or listed write price for writes. Not verified: that their caches hit this way. Gemini explicit caching also adds about $0.03 to $0.05 of storage for one hour.

| Model | No caching | With caching | Sessions per $100, with caching |
| --- | --- | --- | --- |
| Claude Opus 5.5 | $3.91 | $0.60 | about 168 |
| Claude Sonnet 5.5 | $1.95 | $0.30 | about 336 |
| Claude Haiku 4.5 | $0.98 | $0.19 | about 518 |
| Claude Haiku 5.5 | $0.098 | $0.019 | about 5,180 |
| Gemini 3.8 Flash, until 2026-12-31 | $0.73 | $0.14 | about 740 |
| Gemini 3.8 Flash, from 2027-01-01 | $1.47 | $0.27 | about 370 |
| Gemini 3.1 Flash-Lite | $0.25 | $0.047 | about 2,130 |
| OpenAI GPT-6.1 Terra | $0.98 | $0.15 | about 670 |
| OpenAI GPT-6 Luna | $0.098 | $0.019 | about 5,180 |

### Larger material

Take a 60-page material at 120,000 tokens, with the same 20 turns. Total input is 2,537,000 tokens. Cache writes are 131,600 tokens, and cache reads are 2,405,400 tokens.

- Claude Sonnet 5.5: $5.15 without caching, $0.65 with caching.
- Claude Haiku 5.5: every request is over 100,000 tokens, so the higher prices apply. It costs $1.29 without caching and $0.22 with caching.

### Add-ons and sensitivity

- Web search for citations: each search costs $0.01 on the Claude API. Three searches add $0.03, plus the result tokens. The result tokens stay in the history for later turns. This estimate does not count them.
- Tokenizer: the 1,500 to 3,000 tokens per page figure does not name a tokenizer. If Sonnet 5.5 produces 30 percent more tokens, the cached session costs about $0.39.
- Token counts differ across providers for the same text. The cross-provider table uses the same counts for all models, so treat it as a rough comparison.
- Extended thinking adds output tokens at the output price. This estimate counts none.

## What this means for Suro

- The owner's expectation is half right. The Max plan login cannot serve learners. The $100 of monthly API credits can, through an API key in a Console organization.
- A Console organization with auto-reload off gives a hard monthly cap. That fits the v1 spending cap requirement at the account level. Suro still needs its own per-learner cap.
- Prompt caching is not optional for a tutor that resends the material. It cuts the Sonnet 5.5 session cost by about 85 percent.
- Model choice drives cost more than any other factor. A test of tutoring quality on Haiku 5.5 versus Sonnet 5.5 would show whether the 15-times saving is usable.
- BYOK adds friction for testers. Linked billing removes the pasted key, but learners still need a funded Console organization.
- A free Gemini tier is a possible fallback for the first tests. Its data terms conflict with learner materials that hold personal information.

## Follow-up questions

- Does Anthropic confirm that Max monthly API credits may pay for an app that serves the public? Ask Anthropic support to get this in writing.
- Will Suro send the material as extracted text or as a PDF? PDF page images add up to 1,568 tokens per page.
- How long is a real session, and how long do learners pause between turns? Pauses over 5 minutes cause cache misses.
- Does a smaller model teach well enough? This needs a quality test on real materials.
- Can Philippine learners pay Anthropic or OpenRouter with the cards they have?
- What are the Gemini free-tier limits for the owner's account, and is the free tier open in the Philippines?

## Sources

All read on 2026-11-26.

- Anthropic Consumer Terms of Service (effective Oct 8, 2025): <https://www.anthropic.com/legal/consumer-terms>
- Anthropic Commercial Terms of Service: <https://www.anthropic.com/legal/commercial-terms>
- Anthropic Supplemental Credit Terms (effective Mar 4, 2024): <https://www.anthropic.com/legal/credit-terms>
- Anthropic Usage Policy (effective Nov 12, 2026): <https://www.anthropic.com/legal/aup>
- Anthropic supported countries and regions: <https://www.anthropic.com/supported-countries>
- Claude Code legal and compliance: <https://code.claude.com/docs/en/legal-and-compliance>
- Claude Agent SDK overview: <https://code.claude.com/docs/en/agent-sdk/overview>
- Claude plans and pricing: <https://claude.com/pricing>
- Claude API pricing: <https://platform.claude.com/docs/en/about-claude/pricing>
- Claude API rate limits and spend limits: <https://platform.claude.com/docs/en/api/rate-limits>
- Claude API overview: <https://platform.claude.com/docs/en/api/overview>
- Claude API authentication: <https://platform.claude.com/docs/en/manage-claude/authentication>
- Claude PDF support: <https://platform.claude.com/docs/en/build-with-claude/pdf-support>
- Monthly API credits for Max, Ultra, and Team plans: <https://support.claude.com/en/articles/17154008-monthly-api-credits-for-max-ultra-and-team-plans>
- Use the Claude Agent SDK with your Claude plan: <https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan>
- Why is Claude API usage billed separately: <https://support.claude.com/en/articles/9876003-i-have-a-paid-claude-subscription-pro-max-team-or-enterprise-plans-why-is-claude-api-usage-billed-separately-from-my-paid-claude-plan>
- How do I pay for my Claude API usage: <https://support.claude.com/en/articles/8977456-how-do-i-pay-for-my-claude-api-usage>
- Offer linked billing in your platform: <https://support.claude.com/en/articles/18467110-offer-linked-billing-in-your-platform>
- Use your Claude Platform account in a third-party app: <https://support.claude.com/en/articles/18467148-use-your-claude-platform-account-in-a-third-party-app>
- Gemini API pricing: <https://ai.google.dev/gemini-api/docs/pricing>
- Gemini API Additional Terms of Service: <https://ai.google.dev/gemini-api/docs/terms>
- OpenAI API pricing: <https://developers.openai.com/api/docs/pricing>
- OpenRouter FAQ: <https://openrouter.ai/docs/faq>
- OpenRouter limits: <https://openrouter.ai/docs/api_reference/limits>
- OpenRouter OAuth: <https://openrouter.ai/docs/guides/overview/auth/oauth>
- Groq rate limits: <https://console.groq.com/docs/rate-limits>

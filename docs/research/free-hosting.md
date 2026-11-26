# What free or near-free hosting can support for Suro

Research for issue 6. All pages were read on 2026-11-26 unless a row says otherwise.

## Short answer

You can host all of Suro for $0, but no single free plan covers every need well.

- The hardest need is a long AI response. The documented stream limits on free plans fall into four groups:
  - No limit or 15 minutes and more: Cloudflare Workers, Railway (15 min), Render (100 min), and Google Cloud Run (60 min).
  - Up to 5 minutes: Vercel Hobby functions.
  - Up to 150 seconds: Supabase Edge Functions.
  - 100 seconds or less: Koyeb (100 s) and Netlify Functions (60 s).
- Google Cloud Run needs a billing account with a card.
- The second hardest need is a material of several megabytes. Vercel and Netlify functions cap the request body near 4.5 MB.
- Python reduces the options. Netlify has no Python runtime. Cloudflare runs Python through Pyodide, and the Free plan has a 10 ms CPU limit per request.
- Database, file storage, and sign-in have generous free plans at Supabase, Neon, Cloudflare, Firebase, and Clerk.

## How to read this file

- "Stream" means a server-sent events or chunked HTTP response that stays open while the AI model writes.
- Streaming support comes from each provider's documented limits. Nothing was deployed or tested, because the research rules do not allow a sign-up.
- "Not verified" means the pages read do not state the fact. It is not a finding that the limit is absent.
- "Not checked" means no page that could state the fact was read.
- Terms of service were read only for Vercel. For every other provider, commercial-use rules and country rules are not checked.
- "Philippines" means whether a learner or the owner in the Philippines can use the service. No page read names the Philippines as excluded.

## Backend comparison

This table compares the three limits that decide most choices for Suro.

| Option | Free plan today | Longest documented stream | Request body cap | Python | TypeScript | Sleep or cold start | At the free limit | Card needed |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Vercel Hobby functions | Yes | 300 s | 4.5 MB (20 MiB in beta) | Yes | Yes | Not verified | Feature stops for 30 days | Not verified |
| Netlify Free functions | Yes | 60 s | 6 MB, about 4.5 MB for binary | No | Yes | Not verified | All projects pause | Not verified |
| Cloudflare Workers Free | Yes | No wall-clock limit while the client stays connected | 100 MB | Yes, through Pyodide | Yes | Python Workers load a memory snapshot made at deploy | Error 1027 until midnight UTC | Not verified |
| Render Free web service | Yes | 100 min | Not verified | Yes | Yes | Sleeps after 15 min idle; wakes in about 1 min | Free services suspend | No |
| Railway Free | Yes | 15 min if data keeps flowing | No size cap stated; upload must finish in 5 min | Yes | Yes | Sleeps after 5 to 10 min with no outbound traffic | Services stop | No |
| Koyeb free Instance | Yes, inside a paid-account sign-up | 100 s | Not verified | Yes | Yes | Scales to zero after 1 hour idle | Bills on Starter | Yes, with a $29 hold |
| Google Cloud Run | Free tier inside a billing account | 60 min (default 5 min) | 32 MiB on HTTP/1 | Yes | Yes | Scales to zero; cold start time not verified | Bills at standard rates | Yes |
| Fly.io | No free plan for new accounts | Not verified | Not verified | Yes | Yes | Optional auto-stop | Bills | Yes |
| Supabase Edge Functions | Yes | 150 s | Not verified | No | Yes | Not verified | Not verified | Not verified |

## Frontend hosting

The frontend is a static React build, so every option below can serve it.

### Vercel Hobby

- Free plan: yes. The Hobby plan "is free" ([Vercel Hobby plan](https://vercel.com/docs/plans/hobby)).
- Included each month: 100 GB Fast Data Transfer, 1,000,000 CDN requests, and 1,000,000 function invocations ([Vercel Hobby plan](https://vercel.com/docs/plans/hobby)).
- At the limit: "you will have to wait until 30 days have passed before you can use the feature again" ([Vercel Hobby plan](https://vercel.com/docs/plans/hobby)).
- Hobby accounts "cannot purchase additional usage" ([Vercel pricing](https://vercel.com/pricing)).
- Commercial use: the terms say "You shall only use the Services under a Hobby plan for your personal or non-commercial use" ([Vercel terms](https://vercel.com/legal/terms)).
- The terms also allow Vercel to "shut down and terminate projects or deployments using the Hobby plan without notice" ([Vercel terms](https://vercel.com/legal/terms)).
- Credit card: Not verified. The pricing page does not state it.
- Philippines: the terms bar persons that are targets of sanctions ([Vercel terms](https://vercel.com/legal/terms)). Not verified beyond that.

### Netlify Free

- Free plan: yes, "$0 forever" with a "300 credit limit" each month ([Netlify pricing](https://www.netlify.com/pricing/)).
- One credit pool covers deploys, compute, bandwidth, and requests ([Netlify pricing](https://www.netlify.com/pricing/)).
- At the limit: a site "enters a paused state until the start of the next billing cycle." Also, "if one project exceeds its limits, all projects on your account will be paused" ([Netlify pricing](https://www.netlify.com/pricing/)).
- The Free plan has "hard monthly limits that cannot be exceeded or incur any costs" ([Netlify pricing](https://www.netlify.com/pricing/)).
- Credit card and commercial use: Not verified. The pricing page does not state either.

### Cloudflare Pages and Workers static assets

- Pages Free allows 500 builds per month and one build at a time ([Pages limits](https://developers.cloudflare.com/pages/platform/limits/)).
- A Pages site can hold 20,000 files on the Free plan. One file can be up to 25 MiB ([Pages limits](https://developers.cloudflare.com/pages/platform/limits/)).
- For Workers, "Requests to static assets are free and unlimited" ([Workers pricing](https://developers.cloudflare.com/workers/platform/pricing/)).
- Credit card, commercial use, and Philippines: Not verified.

### Firebase Hosting (Spark plan)

- Spark plan: "No payment method needed" ([Firebase pricing](https://firebase.google.com/pricing)).
- Hosting on Spark: 10 GB storage and 360 MB per day of data transfer ([Firebase pricing](https://firebase.google.com/pricing)).
- Firebase App Hosting needs a billing account ([Firebase pricing](https://firebase.google.com/pricing)).

### Render static sites

- Static sites "are free to deploy on Render." They count against the monthly bandwidth and pipeline minutes ([Render free plan](https://render.com/docs/free)).

## Backend hosting

### Vercel functions (Python and TypeScript)

- Duration on Hobby: 300 s default and maximum ([function duration](https://vercel.com/docs/functions/configuring-functions/duration)).
- The duration "includes time spent processing the request and sending the response, including streamed responses" ([function limits](https://vercel.com/docs/functions/limitations)).
- Result: a stream can run for up to 5 minutes. A longer stream ends with a 504 error ([function limits](https://vercel.com/docs/functions/limitations)).
- Request body: "up to 4.5 MB by default." Up to 20 MiB is "in Public Beta for the Node.js, Python, and Bun runtimes" ([function limits](https://vercel.com/docs/functions/limitations)).
- Response body: 4.5 MB, "except for streamed responses" ([function limits](https://vercel.com/docs/functions/limitations)).
- Memory and CPU on Hobby: 2 GB and 1 vCPU ([function limits](https://vercel.com/docs/functions/limitations)).
- Hobby includes 4 CPU-hours of Active CPU and 360 GB-hours of provisioned memory ([Vercel Hobby plan](https://vercel.com/docs/plans/hobby)).
- Waiting on an AI model "does not count towards active CPU time" ([function limits](https://vercel.com/docs/functions/limitations)).
- Python: FastAPI runs as an ASGI `app`. Python 3.12, 3.13, and 3.14 are available ([Python runtime](https://vercel.com/docs/functions/runtimes/python)).
- Python streaming: "Vercel Functions support streaming responses when using the Python runtime" ([Python runtime](https://vercel.com/docs/functions/runtimes/python)).
- Default region is `iad1` (United States), which you can change ([function limits](https://vercel.com/docs/functions/limitations)). Asian regions include `sin1` and `hkg1` ([Vercel regions](https://vercel.com/docs/regions)). Whether Hobby can select them: Not verified.
- Cold start on Hobby: Not verified. The Pro plan lists "Cold start prevention" as a Pro feature ([Vercel pricing](https://vercel.com/pricing)).

### Netlify Functions (TypeScript only)

- Synchronous limit: 60 seconds, not configurable ([function configuration](https://docs.netlify.com/build/functions/configuration.md)).
- Streaming: "Streaming functions have a 60-second execution limit and a 20 MB response size limit" ([functions API](https://docs.netlify.com/build/functions/api.md)).
- Result: a stream of one minute or more does not fit.
- Request body: 6 MB buffered. Binary uploads are Base64-encoded, "effectively reducing the binary request payload size limit to 4.5 MB" ([function configuration](https://docs.netlify.com/build/functions/configuration.md)).
- Background functions run up to 15 minutes, but their payload limit is 256 KB ([function configuration](https://docs.netlify.com/build/functions/configuration.md)).
- Languages: the docs show TypeScript, JavaScript, and Go. No Python runtime appears ([get started](https://docs.netlify.com/build/functions/get-started.md), [function configuration](https://docs.netlify.com/build/functions/configuration.md)).
- Region `sin` (Singapore) is selectable ([function configuration](https://docs.netlify.com/build/functions/configuration.md)).
- Edge Functions: 50 ms CPU per request and a 40 s "Response header timeout" ([Edge Functions limits](https://docs.netlify.com/build/edge-functions/limits.md)). A total stream duration for Edge Functions: Not verified.

### Cloudflare Workers (TypeScript and Python)

- Free plan: 100,000 requests per day, 10 ms CPU time, and 128 MB memory ([Workers limits](https://developers.cloudflare.com/workers/platform/limits/)).
- The daily limit resets "at midnight UTC." Past it, "Cloudflare returns Error 1027" ([Workers limits](https://developers.cloudflare.com/workers/platform/limits/)).
- CPU time excludes waiting: "Waiting on network requests ... does not count toward CPU time" ([Workers limits](https://developers.cloudflare.com/workers/platform/limits/)).
- Duration: "There is no hard limit on duration for HTTP-triggered Workers. As long as the client remains connected, the Worker can continue ... streaming a response body" ([Workers limits](https://developers.cloudflare.com/workers/platform/limits/)).
- Request body: 100 MB on a Free Cloudflare domain plan ([Workers limits](https://developers.cloudflare.com/workers/platform/limits/)).
- Subrequests: 50 per request on Free ([Workers limits](https://developers.cloudflare.com/workers/platform/limits/)).
- Workers Paid costs "a minimum charge of $5 USD per month" ([Workers pricing](https://developers.cloudflare.com/workers/platform/pricing/)). It raises CPU time to "5 min (default: 30 seconds)" ([Workers limits](https://developers.cloudflare.com/workers/platform/limits/)).
- Python: Python Workers run on Pyodide, "CPython ... compiled to WebAssembly" ([how Python Workers work](https://developers.cloudflare.com/workers/languages/python/how-python-workers-work/)).
- Python packages include FastAPI ([Python Workers](https://developers.cloudflare.com/workers/languages/python/), [packages](https://developers.cloudflare.com/workers/languages/python/packages/)).
- Python cold start: Cloudflare snapshots the Worker memory at deploy time "avoiding expensive initialization time" ([how Python Workers work](https://developers.cloudflare.com/workers/languages/python/how-python-workers-work/)).
- Risk: whether a FastAPI Python Worker that reads a several-megabyte material fits in 10 ms of CPU is Not verified. Treat Python on Workers Free as unproven.

### Render (Python and TypeScript)

- Free web services exist for "Node.js, Python, Rails, etc." ([Render free plan](https://render.com/docs/free)).
- Render says: "Do not use them for production applications." They suit "a hobby project" ([Render free plan](https://render.com/docs/free)). This is a production warning, not a commercial-use ban.
- Sleep: a Free web service spins down after "15 minutes without receiving any inbound traffic." Spin-up "takes about one minute" ([Render free plan](https://render.com/docs/free)).
- Hours: 750 Free instance hours per workspace per month. After that, Render "suspends all of your Free web services" ([Render free plan](https://render.com/docs/free)).
- Memory: the Free instance has 512 MB RAM ([Render pricing](https://render.com/pricing)).
- Files: the filesystem is ephemeral. Uploaded files "are lost every time the service redeploys, restarts, or spins down" ([Render free plan](https://render.com/docs/free)).
- Stream duration: "Render allows responses to take up to 100 minutes for HTTP requests" ([Render vs Heroku](https://render.com/docs/render-vs-heroku-comparison)). This comes from a comparison page, not a limits reference page.
- Request body cap: Not verified.
- Credit card: "This tutorial uses free Render resources. No payment is required" ([Render docs index](https://render.com/docs/llms-full.txt)).
- Without a card, exceeding bandwidth "suspends your services for the remainder of the month" ([Render docs index](https://render.com/docs/llms-full.txt)).
- Render may also suspend a Free service that sends "an uncommonly high volume of traffic," including calls to external APIs ([Render free plan](https://render.com/docs/free)).
- Paid step: 512 MB RAM for $7 per month ([Render pricing](https://render.com/pricing)).

### Railway (Python and TypeScript)

- Free plan: "$0 plan: $1 of resource usage each month, no credit card, services that sleep when idle" ([Railway free plan](https://docs.railway.com/pricing/free-plan)).
- Limits: 1 project, up to 3 services, each up to 1 vCPU and 0.5 GB RAM ([Railway free plan](https://docs.railway.com/pricing/free-plan)).
- Sleep: "A service sleeps after 5 to 10 minutes without outbound traffic and wakes on the next request" ([Railway free plan](https://docs.railway.com/pricing/free-plan)).
- Budget: $1 covers about "100 MB, around the clock" of RAM. "When the $1 runs out, your services stop" ([Railway free plan](https://docs.railway.com/pricing/free-plan)).
- Stream duration: "HTTP requests can run for up to 15 minutes if data keeps transferring." With no data for 5 minutes, the request closes ([Railway specs and limits](https://docs.railway.com/networking/public-networking/specs-and-limits)).
- Request body: "Request bodies must finish uploading within 5 minutes." No size cap is stated ([Railway specs and limits](https://docs.railway.com/networking/public-networking/specs-and-limits)).
- Region: on Free, "Railway picks the region" ([Railway free plan](https://docs.railway.com/pricing/free-plan)).
- New accounts get a 30-day trial with a one-time $5 grant ([Railway free plan](https://docs.railway.com/pricing/free-plan)).
- Paid step: Hobby is "$5 a month with $5 of usage included" and runs services always on ([Railway free plan](https://docs.railway.com/pricing/free-plan)).

### Koyeb (Python and TypeScript)

- Free Instance: 512 MB RAM, 0.1 vCPU, and 2 GB SSD. One per organization ([Koyeb instances](https://www.koyeb.com/docs/reference/instances)).
- Regions: "either Frankfurt or Washington, D.C." ([Koyeb instances](https://www.koyeb.com/docs/reference/instances)).
- Sleep: Free Instances "scale down to zero when they don't receive any traffic for 1 hour" ([Koyeb instances](https://www.koyeb.com/docs/reference/instances)).
- Koyeb says Free Instances "should not be used for production applications" ([Koyeb instances](https://www.koyeb.com/docs/reference/instances)).
- Stream duration: "The connection timeout for HTTP requests is set to 100 seconds." Only "WebSockets and gRPC streams" get 12 hours ([Koyeb edge network](https://www.koyeb.com/docs/reference/edge-network)).
- Result: a server-sent events stream longer than 100 seconds does not fit, unless Koyeb treats it as exempt. That exemption is Not verified.
- Credit card: Koyeb requires one and places "a $29 pre-authorization hold" ([Koyeb pricing FAQ](https://www.koyeb.com/docs/faqs/pricing)).
- Sign-up charge: "the selected plan is Pro, so you will be charged the pro-rated amount for Pro." You can then downgrade to Starter ([Koyeb pricing FAQ](https://www.koyeb.com/docs/faqs/pricing)).
- Request body cap: Not verified.

### Google Cloud Run (Python and TypeScript)

- Free tier with request-based billing: 180,000 vCPU-seconds, 360,000 GiB-seconds, and 2 million requests per month ([Cloud Run pricing](https://cloud.google.com/run/pricing)).
- The free tier "is applied as a spending based discount using Tier 1 pricing" ([Cloud Run pricing](https://cloud.google.com/run/pricing)).
- A billing account is required ([Google Cloud free features](https://docs.cloud.google.com/free/docs/free-cloud-features)).
- Card: at Free Trial sign-up "you must provide a credit card or other payment method" ([Google Cloud free features](https://docs.cloud.google.com/free/docs/free-cloud-features)).
- Usage above the free tier "is billed at standard rates" ([Google Cloud free features](https://docs.cloud.google.com/free/docs/free-cloud-features)). No hard stop appears on the pages read.
- Timeout: default 5 minutes, up to 60 minutes ([request timeout](https://docs.cloud.google.com/run/docs/configuring/request-timeout)).
- Google recommends retries and reconnect tolerance for a timeout over 15 minutes ([request timeout](https://docs.cloud.google.com/run/docs/configuring/request-timeout)).
- Request body: 32 MiB on an HTTP/1 server; no limit on HTTP/2 ([Cloud Run quotas](https://docs.cloud.google.com/run/quotas)).
- Response body: the 32 MiB limit applies only "if not using Transfer-Encoding: chunked or streaming" ([Cloud Run quotas](https://docs.cloud.google.com/run/quotas)).
- Billing: an instance bills while "at least one request is being processed" ([Cloud Run pricing](https://cloud.google.com/run/pricing)). A long stream uses the free tier for its full length.
- Region: Singapore (`asia-southeast1`) is available ([Cloud Run pricing](https://cloud.google.com/run/pricing)).
- Cold start time: Not verified.

### Fly.io (Python and TypeScript)

- Free plan: no. Organizations on plans "discontinued in October 2024 keep the free allowances" ([Fly.io pricing](https://docs.fly.io/about/pricing)).
- Card: "All organizations (except for Linked Organizations) require a credit card on file" ([Fly.io pricing](https://docs.fly.io/about/pricing)).
- Cost: one `shared-cpu-1x` 256 MB Machine costs $2.19 per 30 days. A 512 MB Machine costs $3.69 per month ([Fly.io pricing](https://docs.fly.io/about/pricing)).
- Machines can auto-stop when idle, which lowers the cost ([Fly.io pricing](https://docs.fly.io/about/pricing), [Fly Proxy](https://docs.fly.io/reference/fly-proxy)).
- Stream duration and request body cap: Not verified.

## Database

### Supabase Free

- Price: $0. "Free projects are paused after 1 week of inactivity. Limit of 2 active projects" ([Supabase pricing](https://supabase.com/pricing)).
- Database: 500 MB per project, shared CPU, 500 MB RAM ([Supabase pricing](https://supabase.com/pricing)).
- Egress: 5 GB, plus 5 GB cached egress ([Supabase pricing](https://supabase.com/pricing)).
- No automatic backups on Free ([Supabase pricing](https://supabase.com/pricing)).
- Paid step: Pro "From $25 / month" ([Supabase pricing](https://supabase.com/pricing)).
- Credit card, commercial use, behavior at the limit, and Philippines: Not verified.

### Neon Free

- Price: "$0." The page says "no time limits and no credit card required" ([Neon pricing](https://neon.com/pricing)).
- Limits: 100 projects, 100 CU-hours per project each month, and 1 GB storage per project ([Neon pricing](https://neon.com/pricing)).
- Sleep: compute scales to zero "After 5 minutes when inactive" ([Neon pricing](https://neon.com/pricing)).
- Neon also lists Managed Better Auth (up to 60k MAUs per project) and 5 GB of Object Storage on Free ([Neon pricing](https://neon.com/pricing)).
- Wake-up delay after scale to zero: Not verified.

### Cloudflare D1

- Free: 10 databases, 500 MB per database, and 5 GB total ([D1 limits](https://developers.cloudflare.com/d1/platform/limits/)).
- Free: 5 million rows read and 100,000 rows written per day ([Workers pricing](https://developers.cloudflare.com/workers/platform/pricing/)).
- D1 is SQLite-based, not Postgres. Each database "processes queries one at a time" ([D1 limits](https://developers.cloudflare.com/d1/platform/limits/)).

### Render Postgres Free

- "Free Render Postgres databases expire 30 days after creation." Render deletes the data after a 14-day grace period ([Render free plan](https://render.com/docs/free)).
- This makes Render Postgres unsuitable as a lasting free database.

### Railway databases

- Postgres and other databases run on Free as single-node databases with a 0.5 GB volume. They sleep and keep their data ([Railway free plan](https://docs.railway.com/pricing/free-plan)).
- They draw from the same $1 monthly budget ([Railway free plan](https://docs.railway.com/pricing/free-plan)).

### Firebase Firestore (Spark)

- Spark: 1 GiB stored, 50K reads per day, 20K writes per day, and 20K deletes per day ([Firebase pricing](https://firebase.google.com/pricing)).
- Firestore is a NoSQL database, not a SQL database.

## File storage

A tutoring session uploads a material of several megabytes. Each row gives the per-file cap where a page states it.

| Option | Free storage | Per-file or body cap | Egress | Card | Source |
| --- | --- | --- | --- | --- | --- |
| Cloudflare R2 | 10 GB-month | Not verified | Free | Not verified (see the notes) | [R2 pricing](https://developers.cloudflare.com/r2/pricing/) |
| Supabase Storage | 1 GB | 50 MB per file on Free | 5 GB shared | Not verified | [Supabase pricing](https://supabase.com/pricing), [upload limits](https://supabase.com/docs/guides/storage/uploads/file-limits) |
| Vercel Blob | 1 GB | Not verified | 10 GB | Not verified | [Blob pricing](https://vercel.com/docs/vercel-blob/usage-and-pricing) |
| Railway bucket | 10 GB | Not verified | Billed from the $1 | No | [Railway free plan](https://docs.railway.com/pricing/free-plan) |
| Neon Object Storage | 5 GB | Not verified | Not verified | No | [Neon pricing](https://neon.com/pricing) |
| Firebase Cloud Storage | Not on Spark | Not applicable | Not applicable | Yes (Blaze) | [Storage FAQ](https://firebase.google.com/docs/storage/faqs-storage-changes-announced-sept-2024) |

Notes:

- R2 free operations: 1 million Class A and 10 million Class B requests per month ([R2 pricing](https://developers.cloudflare.com/r2/pricing/)).
- R2 sign-up: you "Complete the checkout flow to add an R2 subscription." The docs say "R2 is free to get started" ([R2 get started](https://developers.cloudflare.com/r2/get-started/)).
- R2 card: the official page does not say whether checkout needs a card. A web search returned forum threads that report a card prompt ([community thread](https://community.cloudflare.com/t/why-using-r2-free-tier-involves-giving-card-info/945179)). That thread was not opened, so this is Not verified.
- Vercel Blob on Hobby includes 10,000 simple and 2,000 advanced operations. Uploads count as advanced operations ([Blob pricing](https://vercel.com/docs/vercel-blob/usage-and-pricing)).
- Past the Blob limits, "you will not be able to access Vercel Blob" for 30 days ([Blob pricing](https://vercel.com/docs/vercel-blob/usage-and-pricing)).
- Firebase: "To use Cloud Storage for Firebase ... you must upgrade to the pay-as-you-go Blaze pricing plan." This took effect on February 3, 2026 ([Storage FAQ](https://firebase.google.com/docs/storage/faqs-storage-changes-announced-sept-2024)).
- Render and Koyeb free services have no persistent disk ([Render free plan](https://render.com/docs/free), [Koyeb instances](https://www.koyeb.com/docs/reference/instances)). Store materials in object storage.
- To avoid backend body caps, the browser can upload a material straight to object storage. This is a design option, not a provider claim.

## Sign-in

| Option | Free allowance | Card | Python backend | TypeScript backend | Source |
| --- | --- | --- | --- | --- | --- |
| Supabase Auth | 50,000 MAUs | Not verified | Verify the token in your own code; SDK not checked | Yes | [Supabase pricing](https://supabase.com/pricing) |
| Clerk Hobby | 50,000 monthly retained users per app | No | Yes, official SDK | Yes | [Clerk pricing](https://clerk.com/pricing), [Clerk Python SDK](https://github.com/clerk/clerk-sdk-python) |
| Firebase Authentication | 50K MAUs on Spark | No | Not verified | Yes | [Firebase pricing](https://firebase.google.com/pricing) |
| Neon Managed Better Auth | Up to 60k MAUs per project | No | Not verified | Yes | [Neon pricing](https://neon.com/pricing) |
| Auth.js | Open-source library, no service fee | No | No | Yes | [Auth.js](https://authjs.dev/getting-started) |

Notes:

- Supabase Auth sends only "2 emails per hour with the built-in email provider." You can raise this with custom SMTP ([Supabase Auth rate limits](https://supabase.com/docs/guides/auth/rate-limits)). Email sign-in for strangers needs custom SMTP.
- Clerk Hobby is "Free" with "No credit card required" ([Clerk pricing](https://clerk.com/pricing)).
- Clerk counts a retained user as one who returns "24+ hours after signing up" ([Clerk pricing](https://clerk.com/pricing)).
- Clerk's Python SDK has an `authenticate_request` method for "Django, Flask, and other Python web frameworks" ([Clerk Python SDK](https://github.com/clerk/clerk-sdk-python)).
- Auth.js supports Next.js, Qwik, SvelteKit, and Express ([Auth.js](https://authjs.dev/getting-started)). It has no Python version.
- Auth.js is now maintained by the Better Auth team. They will keep "addressing security patches and urgent issues" ([Better Auth blog](https://better-auth.com/blog/authjs-joins-better-auth)).
- That team "strongly recommend[s] new projects to start with Better Auth" ([Better Auth blog](https://better-auth.com/blog/authjs-joins-better-auth)).

## Other server-side limits worth knowing

- Supabase Edge Functions on Free: 150 s wall clock, 2 s CPU, 256 MB memory, and 150 s idle timeout ([Supabase function limits](https://supabase.com/docs/guides/functions/limits)).
- Supabase Free includes 500,000 Edge Function invocations ([Supabase pricing](https://supabase.com/pricing)).
- Firebase Cloud Functions show "Not applicable" on Spark, so they need Blaze ([Firebase pricing](https://firebase.google.com/pricing)).

## Philippines and region notes

- No page read names the Philippines as excluded. Country rules in the terms are not checked, except for Vercel.
- Vercel's terms bar persons who are targets of sanctions ([Vercel terms](https://vercel.com/legal/terms)).
- Google Cloud says "Depending on your country, you might also need to verify your bank account" ([Google Cloud free features](https://docs.cloud.google.com/free/docs/free-cloud-features)).
- Nearest documented compute regions:
  - Netlify Functions: `sin` (Singapore) ([function configuration](https://docs.netlify.com/build/functions/configuration.md)).
  - Cloud Run: `asia-southeast1` (Singapore) ([Cloud Run pricing](https://cloud.google.com/run/pricing)).
  - Koyeb free Instance: Frankfurt or Washington, D.C. only ([Koyeb instances](https://www.koyeb.com/docs/reference/instances)).
  - Railway Free: Railway picks the region ([Railway free plan](https://docs.railway.com/pricing/free-plan)).
  - Vercel: `sin1` (Singapore) and `hkg1` (Hong Kong). Functions default to `iad1` ([Vercel regions](https://vercel.com/docs/regions)).
  - Render: Singapore is one of five regions ([Render regions](https://render.com/docs/regions)).
  - Supabase: Southeast Asia (Singapore), `ap-southeast-1` ([Supabase regions](https://supabase.com/docs/guides/platform/regions)).
  - Neon: AWS Asia Pacific (Singapore), `aws-ap-southeast-1`. It also hosts Managed Better Auth and Object Storage ([Neon regions](https://neon.com/docs/introduction/regions)).
  - Whether each free plan can pick these Asian regions: Not verified.

## Commercial-use summary

- Vercel Hobby: personal or non-commercial use only ([Vercel terms](https://vercel.com/legal/terms)).
- Render Free and Koyeb free Instance: not for production use ([Render free plan](https://render.com/docs/free), [Koyeb instances](https://www.koyeb.com/docs/reference/instances)). Neither page bans commercial use.
- All other providers: the terms of service are not checked, so a commercial-use rule on the free plan is unknown.

## What this means for Suro

These combinations stay within the limits above. The owner decides in a later ticket.

### Python backend (FastAPI)

1. **Render Free backend, Neon database, R2 or Supabase storage, Clerk or Supabase sign-in, static frontend on Cloudflare Pages.**
   - Cost: $0.
   - Strength: a 100-minute stream limit.
   - Weakness: the first request after 15 idle minutes waits about 1 minute. Render says not to use Free for production.
2. **Railway backend and database, Clerk sign-in, static frontend on Cloudflare Pages.**
   - Cost: $0 on Free, or $5 per month on Hobby for always-on service.
   - Strength: a 15-minute stream limit and no stated body size cap.
   - Weakness: the $1 Free budget is small, and services stop when it runs out.
3. **Cloud Run backend, Supabase for database, storage, and sign-in, static frontend on Cloudflare Pages.**
   - Cost: $0 inside the free tier.
   - Strength: a 60-minute timeout and a 32 MiB body.
   - Weakness: a card is required, and usage past the free tier bills with no hard stop on the pages read. This conflicts with a spending cap.

Vercel Python fits only if every response ends within 5 minutes and the project counts as non-commercial.

### TypeScript backend

1. **All Cloudflare: Workers, static assets, D1, and R2, with Clerk or Better Auth.**
   - Cost: $0, or $5 per month for Workers Paid.
   - Strength: no wall-clock limit on a stream, a 100 MB body, and a hard daily request limit.
   - Weakness: 10 ms CPU per request on Free, and R2 may need a card.
2. **Vercel Hobby with Neon or Supabase and Clerk.**
   - Cost: $0.
   - Strength: one platform for the frontend and the functions.
   - Weakness: 300 s stream cap, a 4.5 MB default body, and the non-commercial rule.
3. **Node server on Render Free or Railway**, with the same database and sign-in choices as the Python options.

## Not verified

- Whether a FastAPI Python Worker that parses a several-megabyte material fits in 10 ms CPU on Cloudflare Workers Free.
- Whether Koyeb's 100-second HTTP timeout applies to an active server-sent events stream.
- Stream duration and request body cap on Fly.io.
- Request body cap on Render and Koyeb.
- Total stream duration for Netlify Edge Functions.
- Cold start or wake time for Vercel Hobby, Cloud Run, and Neon.
- Credit card rules for Vercel Hobby, Netlify Free, Cloudflare Workers Free, and Supabase Free.
- Official confirmation that R2 needs a card.
- Commercial-use rules for every provider except Vercel. Their terms of service are not checked.
- Availability from the Philippines for every provider. Country rules in the terms are not checked, except for Vercel.
- Whether each free plan can pick an Asian region.
- What Supabase Free does when a project passes its limits.
- Render's 100-minute limit on a limits reference page. The figure comes from a Render comparison page.

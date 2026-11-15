# Kill Line

**Every thesis needs a kill line.** Paste any crypto investment thesis in plain words. Kill Line uses **SERV Reasoning** to split it into the claims it depends on, gives each one a metric to track and the number that would prove it wrong, and flags the parts that are just vibes.

Built for **SERV Hackathon 02**, Open Track.

## What it does

For each claim the thesis depends on, Kill Line returns:

- **Kind.** *Testable* means a public metric can refute it on a schedule. *Partial* means only a proxy exists. *Vibes* means no data could ever prove it wrong.
- **Metric.** Something specific enough to compute, like "30-day DEX volume on Base vs the prior 30 days".
- **Source.** Where to pull it from: DefiLlama, Dune, Token Terminal, a chain's RPC, and so on.
- **Kill line.** The pre-committed threshold that would prove the claim wrong. For vibes, it says what would have to be added to make the claim testable.
- **Weight.** How much the whole thesis leans on the claim, from 1 to 5.

For the thesis as a whole, it returns:

- a weight-adjusted **testability score**: how much of the thesis data could actually disprove
- the **weakest link**
- a one-line **verdict**

You can copy the result as a checklist and track it yourself.

## How SERV Reasoning is used

`netlify/edge-functions/kill-line.js` makes one streamed call per analysis to OpenServ's inference API:

- endpoint `https://inference-api.openserv.ai/v1/chat/completions`
- model `gpt-5.4-mini` (override with `SERV_MODEL`), `reasoning_effort: low`
- a structured system prompt that returns the claims as JSON

The API key stays server-side. The output streams to the page as it arrives, so you can watch the reasoning land, and the page then renders the claim cards.

## Run your own

1. Fork this repo and import it into Netlify. There's no build step: the page is served from `public/`.
2. Add the environment variable `SERV_API_KEY` with a key from [console.openserv.ai](https://console.openserv.ai).
3. Deploy, then open the site.

## Layout

```
public/index.html                   the app (no framework, no dependencies)
public/fonts/                       Space Grotesk + Instrument Serif, self-hosted
netlify/edge-functions/kill-line.js the SERV Reasoning call, streamed
```

Analysis, not financial advice. MIT licensed.

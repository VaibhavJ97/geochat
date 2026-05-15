# GeoChat - AI Assistant Grounded in My Master's Thesis

> Ask any question about my M.Sc. thesis on shallow geothermal potential in Germany. Built on Vercel serverless + Google Gemini.

**Live**: [vaibhavj97-geochat.vercel.app](https://vaibhavj97-geochat.vercel.app)

## What this is

A chatbot that answers questions about my thesis using only the actual research data, not generic AI training. The entire thesis content (key findings, methodology, BHE parameters, limitations) is injected into Google Gemini as a system instruction on every request. The user never sees this context but every answer is grounded in it.

## Architecture

```
Browser (vanilla JS chat UI)
   |
   v
Vercel serverless function (api/chat.js, Node.js)
   |
   +-- Inject thesis context as system_instruction
   +-- Send last 6 turns of history with new message
   |
   v
Google Gemini API (gemini-flash-latest)
   |
   v
Reply back to browser
```

## Tech stack

| Layer | What |
|---|---|
| Frontend | Vanilla HTML, CSS, JavaScript (no framework, no build step) |
| Backend | Vercel serverless function in Node.js (ES modules, async/await) |
| AI model | Google Gemini (gemini-flash-latest, temperature 0.4, maxOutputTokens 1500) |
| Hosting | Vercel free tier |
| Cost | **0 EUR/month** at portfolio scale (within Gemini free tier of 1,500 requests/day) |

## Features

- Suggested questions on first load
- Conversation history (last 6 turns sent with each new message for follow-up context)
- Markdown rendering in replies
- Loading state and error handling
- **Embed mode**: append `?embed=1` to the URL to hide nav and footer, used by the floating widget on the thesis page
- Mobile-responsive

## How the context injection works

Rather than letting Gemini answer questions about geothermal energy from its generic training data, the entire thesis is summarized into a system prompt (~3 KB) that gets sent with every request. The prompt contains:

- Key findings with specific numbers (W/m, W, percentages, time horizons)
- Methodology (CMIP6 models, SSP scenarios, MFLS approach, Brent's method)
- BHE physical parameters (depth, conductivity, thermal resistance, etc.)
- Spatial patterns (high-yield and low-yield regions)
- Limitations of the study
- Style and behavior rules ("never invent numbers, use thesis values only", "no em dashes", "no emojis")
- About-GeoChat-itself section so the chatbot can describe its own architecture when asked

The user never sees this. They just ask a question and get an answer rooted in the thesis.

## Why prompt injection instead of RAG or fine-tuning

The thesis context is small enough (~3 KB) to fit comfortably in Gemini's context window with room left for the conversation. Retrieval-augmented generation (RAG) would add embedding storage, vector search, and chunk-retrieval logic for no real benefit at this scale. Fine-tuning would lock in the model and prevent quick prompt iteration. Plain context injection is the right tool for this size of corpus.

## Run locally

```bash
git clone https://github.com/VaibhavJ97/geochat.git
cd geochat
npm install -g vercel
vercel dev
# Open http://localhost:3000
```

You'll need a Gemini API key in `.env.local`:
```
GEMINI_API_KEY=your_key_here
```

Get a free Gemini API key from [ai.google.dev](https://ai.google.dev).

## Deploy your own

1. Fork this repo
2. Connect it to a new Vercel project
3. Add `GEMINI_API_KEY` as an environment variable in Vercel project settings
4. Push to `main`. Vercel auto-deploys.

## Project structure

```
.
├── index.html              # Chat UI
├── assets/
│   ├── style.css           # Styles
│   └── chat.js             # Frontend chat logic
├── api/
│   └── chat.js             # Vercel serverless function (Gemini wrapper)
└── README.md
```

## Limitations

- Free-tier rate limits apply: Gemini allows 1,500 requests/day, Vercel allows 100 GB bandwidth/month
- Context window is the thesis only; the chatbot doesn't know anything outside the thesis
- No fine-tuning, no RAG, no embeddings, just system-prompt context injection
- No conversation persistence between sessions (deliberate, for privacy and simplicity)

## License

MIT

## About me

[Portfolio](https://vaibhavj97.vercel.app) · [Thesis project](https://vaibhavj97-thesis.vercel.app) · [GitHub profile](https://github.com/VaibhavJ97) · [LinkedIn](https://www.linkedin.com/in/vaibhavgeo/)

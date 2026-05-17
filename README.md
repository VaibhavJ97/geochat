# GeoChat - AI Assistant Grounded in My Master's Thesis

> Ask any question about my M.Sc. thesis on shallow geothermal potential in Germany. Built on Vercel serverless + Google Gemini.

**Live**: [vaibhavj97-geochat.vercel.app](https://vaibhavj97-geochat.vercel.app)

## What this is

A chatbot that answers questions about my thesis using only the actual research data, not generic AI training. The entire thesis content (key findings, methodology, BHE parameters, limitations) is injected into Google Gemini as a system instruction on every request. The user never sees this context but every answer is grounded in it.

## How this was built - AI-pair-programming disclosure

This project was built with **AI-assisted development workflows**. Anthropic Claude was my primary pair-programmer for the Vercel serverless function, the prompt engineering, the context injection strategy, the chat UI, and the embed mode. GitHub Copilot handled inline suggestions. ChatGPT helped iterate on the system prompt content (deciding what goes into the thesis context).

**What was mine**: the architecture choice (prompt injection over RAG), the system prompt content rules (no em dashes, never invent numbers, thesis values only), the conversation history strategy (last 6 turns), the model parameters (temperature 0.4, maxOutputTokens 1500), and every line review before deployment.

**What AI accelerated**: the Node.js function structure, async/await patterns, error handling, the markdown rendering in the frontend, and refactoring loops.

There is a meta layer here worth noting: an AI-assisted developer built an AI assistant, using AI as a pair-programmer. The Anthropic "Claude and Claude Code" certification on my portfolio is part of why I'm confident in this workflow.

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
| Development | AI-pair-programming (Claude, ChatGPT, Copilot) with full manual review |
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

## Citations and references

GeoChat is grounded in my M.Sc. thesis. If you reference what GeoChat says, cite the thesis:

```
Jaiswal, V. (2026). Impact of Climate Change on the Geothermal Potential of
Closed Systems Using GIS and Python. M.Sc. Thesis, Karlsruhe Institute of
Technology, Germany. Supervisors: PD Dr. Kathrin Menberg, Dr. Susanne Benz.
```

For the methods that underlie the thesis numbers:
- Rivera, J. A., Blum, P., & Bayer, P. (2017). *Increased ground temperatures in urban areas: Estimation of the technical geothermal potential.* Renewable Energy, 103, 388-400.
- SIA 384/6 standard for ground-source heat exchanger systems.
- CMIP6 (Coupled Model Intercomparison Project Phase 6) climate scenarios accessed via Google Earth Engine.

## Limitations

- Free-tier rate limits apply: Gemini allows 1,500 requests/day, Vercel allows 100 GB bandwidth/month
- Context window is the thesis only; the chatbot doesn't know anything outside the thesis
- No fine-tuning, no RAG, no embeddings, just system-prompt context injection
- No conversation persistence between sessions (deliberate, for privacy and simplicity)
- AI responses can occasionally rephrase content imperfectly; the system prompt instructs Gemini to use thesis values only, but verify before citing

## Disclaimer

GeoChat is a personal project, not a peer-reviewed source. The system is designed to respond from the thesis content, but AI-generated text can still contain errors, paraphrasing artifacts, or unintended omissions. For academic citation, refer directly to the thesis PDF and the underlying references listed above. For engineering decisions (borehole design, installation, permitting), consult licensed professionals - GeoChat is not engineering advice.

## License

MIT

## About me / Contact

- **Email**: vaibhavjaiswal1234@gmail.com
- **Portfolio**: [vaibhavj97.vercel.app](https://vaibhavj97.vercel.app)
- **LinkedIn**: [linkedin.com/in/vaibhavgeo](https://www.linkedin.com/in/vaibhavgeo/)
- **GitHub**: [github.com/VaibhavJ97](https://github.com/VaibhavJ97)
- **Book a 30-min call**: [calendly.com/vaibhavjaiswal1234/30min](https://calendly.com/vaibhavjaiswal1234/30min)
- **Location**: Karlsruhe, Germany

### My other repos

- [Portfolio homepage](https://github.com/VaibhavJ97/VaibhavJ97.github.io) - the front door
- [Master Thesis Project](https://github.com/VaibhavJ97/kit-master-thesis-portfolio) - the thesis this chatbot is built on
- [BHE Recommender](https://github.com/VaibhavJ97/bhe-recommender) - companion tool that uses the same thesis data

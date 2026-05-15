# GeoChat - AI assistant grounded in my Master's thesis

> Ask any question about my Master's thesis (Climate change impact on shallow geothermal potential in Germany). Powered by Google Gemini with the thesis context built in.

**Live site:** [vaibhavj97-geochat.vercel.app](https://vaibhavj97-geochat.vercel.app)
**Embed mode:** [vaibhavj97-geochat.vercel.app/?embed=1](https://vaibhavj97-geochat.vercel.app/?embed=1) (no nav, no footer; used by the thesis page widget)

---

## About this repo

GeoChat is a chatbot grounded in my Master's thesis at KIT, 2026. Rather than guessing or giving generic AI answers, it has the thesis content built in as context: methodology, climate scenarios, key findings, and what the numbers mean.

The chat itself runs on Google Gemini via a Vercel serverless function. The thesis context is injected into every prompt server-side, so users always get answers anchored to the real research.

This repo is one of four in my portfolio:

- [Portfolio homepage](https://vaibhavj97.vercel.app)
- [Master Thesis Project](https://vaibhavj97-thesis.vercel.app)
- **GeoChat** (this repo)
- [BHE Recommender](https://vaibhavj97-bhe.vercel.app)

## Features

- Chat interface with suggested starter questions
- Markdown-rendered responses
- Sticky portfolio nav linking all four sites
- Embed mode (`?embed=1` strips the nav and footer for iframe use)
- "About / How it works / Tech stack" explainer sections after the chat
- Loading indicator and graceful error handling
- Conversation history sent with each request (multi-turn context)

## Tech stack

- **Frontend:** Vanilla HTML / CSS / JavaScript
- **Backend:** [Vercel serverless function](https://vercel.com/docs/functions) (Node.js)
- **AI model:** [Google Gemini](https://aistudio.google.com) (free tier, 1,500 requests / day)
- **Hosting:** Vercel (free tier), auto-deployed from GitHub
- **Total cost to run:** €0 / month

## How it works

1. **You ask a question** - text input on the page sends a POST to `/api/chat` with the message and conversation history.
2. **Thesis context is injected** - the serverless function constructs a Gemini prompt containing:
   - A system message with the thesis context (methodology, results, scenarios)
   - The prior conversation history
   - Your new question
3. **Gemini generates** - the model reads the context and writes an answer grounded in the actual research.
4. **Answer comes back** - the response renders in the chat thread.

The thesis context lives only on the server. The frontend never sees the API key or the system prompt; it only sees the user-facing reply.

## How to reproduce locally

You need a free Gemini API key.

### 1. Get a Gemini API key

Go to [aistudio.google.com](https://aistudio.google.com), sign in with Google, click "Get API key", and copy the key.

### 2. Install Vercel CLI

```bash
npm install -g vercel
```

### 3. Clone and set up

```bash
git clone https://github.com/VaibhavJ97/geochat.git
cd geochat
npm install
```

### 4. Add your API key

Create `.env.local` in the project root:

```
GEMINI_API_KEY=your_key_here
```

### 5. Run

```bash
vercel dev
```

Then open [http://localhost:3000](http://localhost:3000).

## Project structure

```
geochat/
├── index.html          Chat UI, sticky nav, explainer sections, footer
├── api/
│   └── chat.js         Vercel serverless function (Gemini call + context)
├── package.json
├── vercel.json
├── .gitignore
└── README.md
```

## Deploy

1. Push to GitHub
2. Import the repo on [vercel.com/new](https://vercel.com/new)
3. Add `GEMINI_API_KEY` as an environment variable in the Vercel project settings
4. Every push to `main` auto-deploys

## Rate limiting and cost protection

For production use, consider adding:

- Per-IP rate limiting in the serverless function
- A daily request cap with graceful degradation
- [Cloudflare Turnstile](https://www.cloudflare.com/products/turnstile/) (free CAPTCHA) on the public chat
- Hard spending caps in the Gemini dashboard

This deployment uses Gemini's free tier (1,500 req/day) without additional rate limiting, which is sufficient for portfolio traffic.

## AI coding assistance disclosure

The chat interface, prompt-engineering scaffolding, serverless function, embed mode, and the explainer sections were developed with [Claude](https://claude.ai) (Anthropic) as a coding partner. The thesis context that gets injected into every prompt was author-written based on my own research. Google Gemini powers the live chat responses.

## Author

**Vaibhav Jaiswal**
M.Sc. Applied Geosciences, Karlsruhe Institute of Technology, 2026

- Email: vaibhavjaiswal1234@gmail.com
- LinkedIn: [linkedin.com/in/vaibhavgeo](https://www.linkedin.com/in/vaibhavgeo/)
- GitHub: [@VaibhavJ97](https://github.com/VaibhavJ97)
- Portfolio: [vaibhavj97.vercel.app](https://vaibhavj97.vercel.app)

## License

Code released under the MIT License. Thesis content (in the system prompt) remains under standard KIT academic terms; please credit if you reuse this pattern for your own research portfolio.

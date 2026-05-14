# GeoChat

AI chatbot grounded in my Master's thesis at the Karlsruhe Institute of Technology (KIT):
*Climate change impact on shallow geothermal potential in Germany using a single borehole heat exchanger.*

Live: [vaibhavj97-geochat.vercel.app](https://vaibhavj97-geochat.vercel.app) (or your chosen URL)

## What it does

Ask GeoChat questions about the thesis: methodology, scenarios, results, what the numbers mean, what the limitations are. Sample questions:

- What is the geothermal potential in Germany under SSP 5-8.5?
- Compare 50-year and 100-year sustainable extraction rates.
- Explain the FLS (Finite Line Source) method in simple words.
- How much does climate change actually boost BHE output?
- What are the limitations of the model?

## How it works

- **Frontend**: vanilla HTML, CSS, JS. No framework. Matches the visual style of [my portfolio](https://vaibhavj97.vercel.app).
- **Backend**: a single Vercel serverless function (`api/chat.js`) that forwards the user's question to Google Gemini 2.0 Flash, with the thesis context as a system prompt.
- **Knowledge base**: the key findings, methodology, parameters, and numerical results from the thesis are embedded directly in the system prompt. This keeps the answer grounded and avoids hallucinations.

## Architecture

```
user -> static frontend (index.html)
            |
            v
       /api/chat (Vercel serverless function)
            |
            v
       Google Gemini API (with thesis context as system prompt)
```

## Stack

- HTML / CSS / vanilla JS
- Vercel (hosting + serverless functions)
- Google Gemini 2.0 Flash (LLM)

## Run locally

```bash
# install Vercel CLI globally if you don't have it
npm install -g vercel

# clone this repo, then
cd geochat
vercel dev
```

You will need a `GEMINI_API_KEY` environment variable. Set it in `.env.local` for local dev, or in the Vercel dashboard for production.

## Privacy

No conversation is stored. Each request is forwarded to Gemini and the response is returned to the user. The serverless function does not log message content.

## Author

[Vaibhav Jaiswal](https://vaibhavj97.vercel.app)
MSc Applied Geosciences, KIT
[LinkedIn](https://www.linkedin.com/in/vaibhavj97) | [GitHub](https://github.com/VaibhavJ97)

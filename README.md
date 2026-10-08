# Presentation Practice

A presentation rehearsal workspace by Anthony Theunissen.

**Live tool:** https://anthonytheunissen.github.io/presentation-practice/

Upload a PDF or PowerPoint, view slides beside a practice coach, write speaking cues and transitions, rehearse questions, and time your delivery. Choose a speaking route by excluding optional or reference slides. Save and reload a JSON practice plan.

## Uploads and privacy

- PDF: original slide visuals and extracted text.
- PPTX: text view and speaker notes, in presentation order. Pair a PDF from the same version for faithful visuals. Matching slide counts are checked; users must check the version/order.
- Maximum 50 MB and 200 slides. Scanned PDFs with no text cannot supply content for AI; there is no OCR.
- Original files stay in browser memory. No automatic storage, accounts, analytics or server upload. Refresh clears the presentation and edits.
- Saved practice plans contain your cues, questions and notes, not the original deck. Keep sensitive plans private.
- AI is optional and explicitly consented per session. The selected slide’s text and speaker notes, adjacent titles, audience and goal go to the configured coach service and OpenAI. Original PDFs/PPTX files and visuals are not sent.
- No personal Capitec presentation or career evidence is included in this repository.

## AI coaching status

The AI interface and server integration are implemented. **Live AI requires a separately deployed server with an OpenAI API key and an access code. GitHub Pages alone cannot run the server.** Manual coaching works without this connection.

AI drafts provide a main message, transition, challenging question, answer anchors and evidence boundaries. Users review a draft before applying it. The coach does not audit evidence, judge voice delivery or guarantee correct claims.

## Run locally

Requires Node.js 22+; no package installation needed.

```sh
node server.mjs
```

Open http://localhost:8787. For manual-only use, any static HTTP server works. Do not open `index.html` as a file URL: PDF.js uses modules and a web worker.

## Connect AI

Set server environment variables (never put an API key in browser code or GitHub):

- `OPENAI_API_KEY`: your OpenAI project key.
- `AI_ACCESS_CODE`: required invitation code. Keep it out of `config.js` and public source.
- `OPENAI_MODEL`: defaults to `gpt-5-mini`.
- `ALLOWED_ORIGINS`: comma-separated frontend origins; for GitHub Pages use `https://anthonytheunissen.github.io`.
- `AI_DAILY_LIMIT`: defaults to 100 requests per server process per UTC day.
- `PORT`: defaults to 8787.

Deploy `server.mjs` to a Node-capable host, set the above variables in the host’s secret settings, then set `coachEndpoint` in `config.js` to the HTTPS `/api/coach` URL. The service URL is configured by the site owner and is never requested from presenters. Until configured, the interface clearly marks AI as unavailable and disables generation.

The server requires an access code, enforces an origin allowlist, a 40 KB request limit, a 10-request/hour socket-IP limit and a process-wide daily limit. It sends `store:false` to OpenAI and does not log slide text or store requests. `store:false` does not mean the provider has zero retention; see OpenAI’s data policy.

**Beta limits:** counters are in memory and reset on restart; multiple processes have separate counters. Behind a proxy the socket-IP limit may be shared by users. Use a single process for an invitation beta, set provider spend limits, and add durable quotas and proper user authentication before offering open, owner-funded AI access at scale. The access code remains in browser memory only and is excluded from exported plans.

## GitHub Pages

Publish the `main` branch root from repository Settings → Pages. `.nojekyll` enables direct static serving. `server.mjs` is source code on Pages; it is never executed there and contains no secrets.

## Verification

Checked PDF rendering, PowerPoint note pairing, cue editing, route filtering, timers, responsive layout, AI draft/apply with mocked responses, and server input validation, authentication and limits. Live OpenAI generation is not verified until a service key is configured.

## Dependencies

Vendored Mozilla PDF.js 5.6.205 (Apache-2.0) and JSZip 3.10.1 (MIT or GPLv3 dual licence). Their licence notices are in `vendor/`. Maintain and review dependency updates when publishing new versions.

## Licence

Project code: MIT. Third-party dependency licences remain in force.

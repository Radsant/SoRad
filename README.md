# SoRad

**The pocket hypeman.** Tell it anything and it tells you that's so rad.

SoRad is a tiny AI agent with exactly one job. You type what's going on, from shipping a redesign to burning your toast, and it hypes you up in a fresh way every time. It lives inside a retro handheld with a green pixel screen, chiptune sounds, and little pixel celebrations.

**Try it:** [sorad.netlify.app](https://sorad.netlify.app) <!-- update once the site name is set -->

Designed and built by [Radhika Sant](https://radhikasant.com).

## Why

Everyone is building AI agents that do everything. SoRad does one thing and does it with total commitment. It's a playful take on the agent hype, and a small study in designing an agent's personality, limits, and failure modes.

## How it works

```
Browser (index.html)  ->  POST /api/hype  ->  Netlify Function  ->  Claude
                                               |
                                               +-> checks the reply, swaps in a
                                                   saved line if it breaks the rules
```

1. The page sends what you typed (up to 280 characters) to a small backend function.
2. The function sends it to Claude with the SoRad persona rules, and asks for a short JSON answer: is this safe to hype, and what's the reply.
3. Before anything reaches the screen, the function checks the reply. If it's too long, or doesn't say "rad," a saved line is shown instead.
4. The page plays a random pixel animation and a short chiptune.

The API key stays in Netlify's environment variables. It is never sent to the browser or stored in this repo.

## Design decisions

**One job, no exceptions.** SoRad never answers questions, gives advice, or does tasks. Ask it to write your cover letter and it hypes the fact that you're applying.

**It knows when not to hype.** Messages about self-harm, violence, or dangerous plans get a calm, kind reply with no celebration. Crisis messages point to 988, the US crisis line. This check runs in the page, again in the backend, and once more by the model.

**It fails gracefully.** If someone tries to jailbreak it, or the model is slow or down, SoRad falls back to a list of saved "rad" lines. The screen labels these so it's clear what's happening.

**It's hard to run up a bill.** The backend only accepts requests from this site, limits how often each visitor can call it, and caps how long replies can be. The real cost cap is a monthly spend limit on the API account.

**Two voices.** Hype mode cheers you on. Roast mode gives a playful roast, an unexpected compliment, then reluctant encouragement. It roasts the situation, never the person, and drops the roast entirely when someone sounds hurt.

**Built for thumbs.** On phones the handheld fills the screen, the buttons sit at thumb height, the phone buzzes on each hype (Android), and it can be added to the home screen like an app.

**Nostalgia over polish.** Four shades of green, pixel type, physical buttons, and sound you can turn off with SELECT. The animations are drawn live on a canvas, so they match the screen instead of looking like pasted-in GIFs.

## Controls

| Button | What it does |
| --- | --- |
| A or Enter | Hype me |
| B | Clear the screen |
| D-pad up / down | Replay the RAD! or the confetti (arrow keys work too) |
| D-pad middle | Replay a random celebration |
| D-pad left / right | Scroll through ideas to try |
| START | Random idea, instant hype |
| Mode badge (HYPE MODE / ROAST MODE) | Switch between Hype and Roast mode |
| SELECT | Sound on or off |

## Files

| File | What it is |
| --- | --- |
| `index.html` | The handheld page: layout, pixel animations, sound |
| `netlify/functions/hype.mjs` | The backend: persona prompt, safety checks, rate limit, fallback lines |
| `netlify.toml` | Tells Netlify where the page and the function live |
| `manifest.webmanifest`, `icon-*.png` | Lets people add SoRad to their phone's home screen as an app |

## Run your own

1. Fork this repo and import it into Netlify (Add new site > Import an existing project).
2. Add an environment variable named `ANTHROPIC_API_KEY` with your key from [console.anthropic.com](https://console.anthropic.com).
3. Set a monthly spend limit on your API account.
4. Deploy.

Optional environment variables: `MODEL` to use a different Claude model, and `ALLOWED_ORIGINS` (comma-separated) to let other sites, like your portfolio, call the backend.

## What's next

- A Chrome extension version
- An embedded demo on radhikasant.com

## License

MIT. Use it, remix it, give it a new catchphrase. A link back is appreciated.

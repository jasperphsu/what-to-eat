# 🍽️ What to Eat

Plan it · snap it · cook it. A single-file web app that helps you decide what to cook:

- **🗓️ Today** — build today's plan; alarms ring (and buzz on Android) when it's time to cook
- **📸 Snap** — point your iPhone/iPad camera at the fridge; Claude AI spots the ingredients and suggests recipes
- **💬 Ask** — tap a mood (8 emotions!), mark allergies, or type a craving
- **📖 Recipes** — browse everything, search by ingredient
- **👨‍👩‍👧 Family** — picks tuned to each person's tastes and allergies
- **🎡 Spin** — still can't decide? The wheel picks for you

Everything lives in one file: `index.html`. No build step, no dependencies.

## Ship it to GitHub (5 minutes)

1. Create a new **public** repo named `what-to-eat` (no personal names — matches the class style).
2. Upload `index.html` and this `README.md` to the repo root.
3. Repo **Settings → Pages → Source: Deploy from a branch → main / (root) → Save**.
4. After ~1 minute your app is live at `https://YOUR-USERNAME.github.io/what-to-eat/`.

> The live camera **requires HTTPS**, which GitHub Pages provides automatically. Opening the file by double-click still works for everything except the live camera (the "upload a photo" button covers that case).

## Connect Claude (the AI ingredient scanner)

The app calls Claude's vision API directly from the browser. The key is **never stored in the code or the repo** — each device enters its own key once:

1. Get a key at [console.anthropic.com](https://console.anthropic.com) → **API Keys → Create Key**. Set a monthly spend limit while you're there.
2. Open the app → tap **⚙️** in the top bar → paste the key → **Save key**.
3. The key is kept in that browser's `localStorage` only. The 📸 Snap tab now auto-detects ingredients.

How it works (in `index.html`, the `claude()` function): a `fetch` to `https://api.anthropic.com/v1/messages` with the `anthropic-dangerous-direct-browser-access: true` header, which is Anthropic's official opt-in for calling the API from a web page. The captured photo is sent as base64 JPEG with a prompt asking for a JSON array of ingredient names.

**Model:** `claude-opus-4-8` (one line in the `claude()` function — switch to `claude-haiku-4-5` if you want cheaper scans).

### ⚠️ Key safety rules

- Never paste the key into the code or commit it to GitHub — anyone could copy it and spend your credits.
- Anyone you give the key to can use your account: share it only with people you trust (e.g., enter it on class iPads yourself).
- Set a spend limit in the Anthropic console.
- If a key leaks, delete it in the console and make a new one.

### Level-up (optional, later)

The class app *What for Lunch* hides its key behind a tiny **Supabase Edge Function** proxy: the website talks to the proxy, the proxy holds the secret key, locks requests to the site, and rate-limits abuse. That's the "grown-up" architecture if this app ever gets real users — happy to set it up when you're ready.

## Customizing

All in `index.html`:

- **Colors** — the `:root` CSS variables at the top (`--accent` is the pinkish-purple)
- **Recipes** — add to the `RECIPES` array (future idea: add a `collection:` field to group them)
- **Moods** — the `MOODS` array
- **Family** — the `PEOPLE` array
- **Alarm sound** — the `notes` array in `ringBell()`

## Notes

- Alarms ring while the app is open (browsers don't let web pages ring when closed).
- Vibration works on Android; iPhones don't allow web vibration — they get the sound + banner.
- Data (plan, ingredients, moods) is saved per-browser in `localStorage`.

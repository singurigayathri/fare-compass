# Fare Compass

A flight price finder: cheapest-day calendar, one-way vs return comparison, cheapest city to fly into, flight list and booking links. Prices come from Google Flights through SerpApi (US market, USD).

Without a connected key the site runs on **sample prices** and shows a yellow banner saying so.

## Files
- `index.html`: the whole website
- `worker/worker.js`: a Cloudflare Worker that keeps your SerpApi key secret (for GitHub Pages)
- `api/search.js` + `package.json`: the same thing for Vercel (only needed if you host there instead)

## Host on GitHub Pages (free)

### 1. Get a SerpApi key
Sign up at serpapi.com and copy your private API key from the dashboard.

### 2. Create the Cloudflare Worker (it holds the key)
1. Sign up at dash.cloudflare.com (free plan is fine).
2. Go to **Workers & Pages → Create → Create Worker**, name it e.g. `fare-compass`, then **Deploy**.
3. Click **Edit code**, delete everything, paste the contents of `worker/worker.js`, then **Deploy**.
4. Go to the worker's **Settings → Variables and Secrets** and add:
   - `SERPAPI_KEY` as type **Secret**, with your SerpApi key as the value
   - `ALLOWED_ORIGIN` as type **Text**, with value `https://YOUR-GITHUB-USERNAME.github.io` (no slash at the end)
5. Copy the worker URL, e.g. `https://fare-compass.yourname.workers.dev`.

### 3. Put the site on GitHub
1. Open `index.html` and find the line `const WORKER_URL = "";`. Paste your worker URL between the quotes.
2. On GitHub create a new **public** repository, e.g. `fare-compass`, and upload `index.html`. You can upload the other files too; they don't do any harm.
3. In the repo go to **Settings → Pages → Build and deployment**, set Source to **Deploy from a branch**, Branch to **main** and folder to **/ (root)**, then **Save**.
4. After about a minute your site is live at `https://YOUR-GITHUB-USERNAME.github.io/fare-compass/`.

When the yellow "Sample prices" banner is gone, you're on live prices.

## Host on Vercel instead
Import the repo at vercel.com, add the environment variable `SERPAPI_KEY`, redeploy, and leave `WORKER_URL` empty.

## How many searches it uses
Each search uses one calendar call, one flight call, and one more calendar call for each "Also compare" city. "Compare prices" (one-way vs return) uses 2–3 more. Repeat searches are cached for 30 minutes. Your SerpApi dashboard shows usage against your plan.

## Never
- Don't put your SerpApi key in `index.html`. The repo and the site are public, so anyone could copy it.

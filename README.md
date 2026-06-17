# Luxe Dallal — Events & Catering

## Files

- `index.html` — English (landing page)
- `ar.html` — Arabic (RTL)
- `images/` — drop your AI-generated photos here

## Image filenames the site looks for

Place all images inside a folder called `images/` next to the HTML files.

| Filename            | Where it appears                  | Recommended size      |
| ------------------- | --------------------------------- | --------------------- |
| `hero.jpg`          | Hero background (full screen)     | 2400 × 1600 (3:2)     |
| `food.jpg`          | "Food" pillar card                | 1200 × 900  (4:3)     |
| `drinks.jpg`        | "Drinks" pillar card              | 1200 × 900  (4:3)     |
| `setup.jpg`         | "Setup" pillar card               | 1200 × 900  (4:3)     |
| `gallery-1.jpg`     | Gallery — large left tile         | 1200 × 1600 (3:4)     |
| `gallery-2.jpg`     | Gallery — top right tile          | 1000 × 700  (10:7)    |
| `gallery-3.jpg`     | Gallery — middle right tile       | 1000 × 700            |
| `gallery-4.jpg`     | Gallery — bottom right tile       | 1000 × 700            |

If a file is missing, the section gracefully falls back to a warm gradient — nothing breaks. So you can launch with whatever you have and add the rest later.

## Tips

- **Compress before uploading.** Use [squoosh.app](https://squoosh.app) — aim for ~200–400 KB per image. Big files = slow site.
- **JPG, not PNG.** Photos compress much better as JPG.
- **Keep the style consistent.** Same warm gold + dark tones across all images so the gallery feels cohesive.

## AI prompts to generate the images

See the chat where these were drafted, or reuse these:

**hero.jpg** — Editorial catering photography, candlelit long dining table set for an elegant event, warm gold and deep charcoal tones, soft blurred bokeh in background, luxurious tablescape with brass candleholders, dark moody atmosphere, shot from low angle, cinematic, 16:9.

**food.jpg** — Close-up overhead shot of an elegantly plated Middle Eastern fine-dining dish, warm gold light, dark slate background, drizzled sauce, micro herbs, moody editorial food photography, shallow depth of field, 4:3.

**drinks.jpg** — Crystal cocktail glass with amber liquid on a dark marble bar, candlelight glow, brass details, moody nighttime event bar, editorial drink photography, warm tones, 4:3.

**setup.jpg** — Beautifully decorated event tablescape from a 45-degree angle, white linen, gold cutlery, ivory florals, candles, warm ambient light, luxurious wedding or private dinner setup, 4:3.

**gallery-1.jpg through gallery-4.jpg** — Variations: the host plating, guests at table, food close-ups, full room scenes. Same warm-gold-on-dark mood.

## Hosting & Deploy

The site is a **static site hosted on Cloudflare Pages**, served at the custom
domain **https://luxedallal.com** (and the project URL https://luxe-dallal.pages.dev).

### How it works (the whole chain)

1. **Files** — `index.html`, `ar.html`, and `images/` are plain static files.
   There is no server or build step; Cloudflare just serves the files.
2. **Cloudflare Pages** — a Pages project named **`luxe-dallal`** holds the
   uploaded files and serves them from Cloudflare's global CDN (fast everywhere,
   free TLS). Each upload is an immutable "deployment" with its own preview URL.
3. **Custom domain** — `luxedallal.com` is a Cloudflare zone (DNS is on
   Cloudflare). An **apex `CNAME luxedallal.com → luxe-dallal.pages.dev`**
   (Proxied / orange-cloud, using Cloudflare's CNAME flattening) points the
   domain at the Pages project.
4. **HTTPS** — Cloudflare auto-issues and renews the SSL certificate
   (Google Trust Services). Nothing to manage.

### Deploying an update

Deploys are a **manual direct upload** via Wrangler — **`git push` does NOT
deploy** (the repo is not connected to Pages). After editing the HTML:

```sh
# Wrangler is installed in ~/.wrangler-tool (Node 20 via nvm, wrangler v3)
export NVM_DIR="$HOME/.nvm"; . "$NVM_DIR/nvm.sh"; nvm use 20

# Stage only the site files (keeps the .qrvenv / .git out of the upload)
PUB=/tmp/luxe-publish; rm -rf "$PUB"; mkdir -p "$PUB"
cp index.html ar.html "$PUB"/; cp -R images "$PUB"/images

# Upload — creates a new live deployment
cd ~/.wrangler-tool
./node_modules/.bin/wrangler pages deploy "$PUB" \
  --project-name luxe-dallal --branch main --commit-dirty=true
```

The custom domain updates within seconds of a successful deploy.

### Auth / accounts

- **Wrangler login** (`wrangler login`, OAuth) can deploy Pages but has **no DNS
  permission**. DNS changes were done through the **Cloudflare API MCP**
  (the `cloudflare` Claude Code plugin), whose OAuth grants DNS edit.
- Cloudflare account: `khaledabdullahali@gmail.com`.

### Optional next step

To make `git push` auto-deploy, connect the GitHub repo
(`github.com/Khaled-92/bahaa-hammode`) to the Pages project in the Cloudflare
dashboard (Workers & Pages → luxe-dallal → Settings → Builds & deployments →
connect to Git). Until then, run the Wrangler command above.

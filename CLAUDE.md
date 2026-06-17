# Luxe Dallal — Events & Catering (website)

Static marketing site for **Luxe Dallal**, an events & catering business.
Bilingual: English (`index.html`) + Arabic RTL (`ar.html`). No backend, no build step.

## Files
- `index.html` — English landing page
- `ar.html` — Arabic landing page (RTL)
- `images/` — photos + brand logo `yellow-luxe-dallal.png` (gold mark, used in the nav)
- `qr-luxedallal.png` / `qr-luxedallal-gold.png` — QR codes for https://luxedallal.com/
- `.qrvenv/` — Python venv with `qrcode` + `PIL` for regenerating QR codes (gitignored)

## Page structure (both languages)
- Nav with logo (`images/yellow-luxe-dallal.png`)
- Hero
- `01 / Services` — three catering pillars: Food, Drinks, Setup
- `02 / Beyond the table` — seven offering cards (`.offerings` grid): Wedding Guest
  Dresses, Event Venue, Decoration, Entertainment, Staff Service, Beauty Salon, Barber
- `03 / Experience`, `04 / Gallery`, `05 / Occasions`, `06 / Contact`
- Offerings use a responsive `repeat(auto-fill, minmax(270px,1fr))` card grid;
  collapses to one column ≤900px.

## Brand / style
- Colors (CSS vars in each file's `:root`): `--bg #0e0c0a`, `--bg-2 #15110d`,
  `--gold #c9a35a`, `--ink-soft #c9bda3`, `--line rgba(243,233,212,0.12)`
- Fonts: Fraunces (serif headings), system sans for body
- RTL: Arabic page mirrors layout; uses Arabic-Indic numerals (٠١, ٠٢ …) for section labels

## Hosting & deploy — IMPORTANT
- **Cloudflare Pages**, project **`luxe-dallal`**, custom domain **https://luxedallal.com**
  (also https://luxe-dallal.pages.dev). Apex `CNAME luxedallal.com → luxe-dallal.pages.dev`
  (Proxied). HTTPS auto-issued by Cloudflare.
- **`git push` does NOT deploy** — the repo is not (yet) connected to Pages.
  Deploys are a **manual upload via Wrangler**:

  ```sh
  export NVM_DIR="$HOME/.nvm"; . "$NVM_DIR/nvm.sh"; nvm use 20   # wrangler v3 needs Node 20
  PUB=/tmp/luxe-publish; rm -rf "$PUB"; mkdir -p "$PUB"
  cp index.html ar.html "$PUB"/; cp -R images "$PUB"/images
  cd ~/.wrangler-tool && ./node_modules/.bin/wrangler pages deploy "$PUB" \
    --project-name luxe-dallal --branch main --commit-dirty=true
  ```

  (Wrangler is installed in `~/.wrangler-tool`, not globally — global npx wants Node 22.)
- **DNS/domain changes** can't be done with the `wrangler login` token (no DNS scope).
  Use the Cloudflare API MCP (the `cloudflare` Claude Code plugin) instead.
- Cloudflare account: `khaledabdullahali@gmail.com`.

## Git
- Remote: `github.com/Khaled-92/bahaa-hammode`, branch `main` (repo name predates the
  rebrand from "Bahaa Hammode" to "Luxe Dallal").
- Commit + push for history, but remember it does **not** trigger a deploy (see above).

## Regenerate QR codes
```sh
.qrvenv/bin/python -c "import qrcode; qr=qrcode.QRCode(error_correction=qrcode.constants.ERROR_CORRECT_H,box_size=20,border=4); qr.add_data('https://luxedallal.com/'); qr.make(fit=True); qr.make_image(fill_color='#0e0c0a',back_color='#ffffff').convert('RGB').save('qr-luxedallal.png')"
```

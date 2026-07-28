# AIRON Landing Page — Deploy útmutató

**Idő**: kb. 30 perc a nulláról → élő weboldal a saját Porkbun-domain-en, HTTPS-sel.
**Költség**: 0 Ft (Vercel ingyenes tier, korlátlan bandwidth landing page-hez).

Ez a mappa:
- `index.html` — magyar landing page (1 fájl, minden benne)
- `en/index.html` — angol nyelvű verzió (`/en/` URL alatt)
- `airon-logo.svg` — a nagy logo (standalone használatra)
- `favicon.svg` — a böngésző-fülre kerülő ikon
- `README.md` — ez a fájl

**Nincs build-lépés**, nincs `npm install`, semmi. Tiszta HTML + Tailwind CDN-ből. Bárhol lefut.

**A landing page tartalma**:
- HU + EN verzió (nav-ban `HU · EN` váltó)
- 3 sztenderd csomag (Starter / Standard / Enterprise) — árak NÉLKÜL, csak feature-lista
- 4. **TAILORED** opció — egyedi Discovery meeting-alapú csomag
- Minden CTA "Kérj ajánlatot" / "Discovery meeting" → info@fortytech.hu-ra megy

---

## LÉPÉSRŐL LÉPÉSRE

### 1. lépés — GitHub account (ha még nincs)

Ha még nincs GitHub fiókod:
1. Menj a [github.com](https://github.com) oldalra
2. `Sign up` — email + jelszó + username
3. Erősítsd meg az emailt

Ha van GitHub-od, jó — folytasd a 2. lépésre.

### 2. lépés — Új repository létrehozás

1. GitHub-on, jobb felső `+` → `New repository`
2. **Repository name**: `airon-landing` (vagy amit szeretnél)
3. **Public** vagy **Private** — mindkettő működik (Public-ba jobb: gyorsabb deploy)
4. NE pipáld be az `Add a README` / `Add .gitignore` opciókat — üresen hagyd
5. `Create repository`

### 3. lépés — Fájlok feltöltése GitHub-ra

A legegyszerűbb: **böngészőn keresztül**, semmi `git` parancs nem kell.

1. Az újonnan létrehozott repo oldalán: `uploading an existing file` link
2. Húzd be (drag-drop) **ezt a 4 fájlt + `en/` mappát**:
   - `index.html` (HU verzió)
   - `en/index.html` (EN verzió — mappával együtt drag-drop)
   - `airon-logo.svg`
   - `favicon.svg`
3. Lent: `Commit changes` gomb

**FONTOS**: az `en/` mappát is töltsd fel! Ha csak az `index.html`-eket viszed és nincs mappa-struktúra, az `en/` link 404-et ad. GitHub-on drag-drop-nál ha teljes mappát húzol, a szerkezet is megőrződik.

Kész — a fájlok a GitHub-on vannak.

**Alternatíva** ha ismersz git-et:
```bash
git clone https://github.com/USERNAME/airon-landing.git
cd airon-landing
# másold ide a 3 fájlt
git add .
git commit -m "Initial landing page"
git push
```

### 4. lépés — Vercel account + deploy

1. Menj a [vercel.com](https://vercel.com)-re
2. `Sign Up` — **válaszd a "Continue with GitHub"** opciót (így nem kell újra jelszót csinálnod)
3. Autorizáld a Vercel-t GitHub-hoz
4. Vercel-en a dashboard-on: `Add New...` → `Project`
5. `Import Git Repository` szekcióban keresd meg `airon-landing` repo-t → `Import`
6. **Framework Preset**: `Other` (mert tiszta HTML)
7. **Build Command**: hagyd üresen
8. **Output Directory**: hagyd üresen (default: `./`)
9. `Deploy` gomb

Vercel 30 másodpercen belül feldeploy-olja. Kapsz egy URL-t: `airon-landing-xyz.vercel.app`

**Nézd meg** — élő weboldal, HTTPS-sel, ingyen. Ez az alap.

### 5. lépés — Saját domain rákötése (Porkbun → Vercel)

Most kössük rá a saját domain-t (pl. `airon.hu`).

#### 5.1 — Vercel oldalon domain hozzáadás

1. A Vercel projekt oldalán: `Settings` → `Domains`
2. Írd be a domain-t: `airon.hu` (a `www.` prefix nélkül) → `Add`
3. Vercel megmutatja mit kell beállítani a DNS-en. Két lehetőség:
   - **Opció A** (ajánlott): `A record` a root-domain-re (`airon.hu`)
     - Type: `A`
     - Host: `@`
     - Value: `76.76.21.21`
   - **Opció B**: Ha inkább `www.airon.hu`-ra akarod:
     - Type: `CNAME`
     - Host: `www`
     - Value: `cname.vercel-dns.com`

**Ajánlás**: mindkettőt csináld meg — hogy a root és a www is működjön.

#### 5.2 — Porkbun-on DNS record beállítás

1. Menj a [porkbun.com](https://porkbun.com)-ra, `Sign In`
2. `Domain Management` → keresd meg az `airon.hu`-t → `DNS`
3. Töröld a meglévő A/CNAME record-okat amit Porkbun default beállított (parking page-ek)
4. Adj hozzá új record-okat:

   **Record 1 — root domain**:
   - Type: `A`
   - Host: (üresen hagyd, vagy `@`)
   - Answer: `76.76.21.21`
   - TTL: `600`

   **Record 2 — www subdomain**:
   - Type: `CNAME`
   - Host: `www`
   - Answer: `cname.vercel-dns.com`
   - TTL: `600`

5. `Save` (vagy `Add DNS Record` — Porkbun UI-jától függ)

#### 5.3 — Várakozás + SSL

- **DNS terjedés**: 5-30 perc (max 24 óra)
- **SSL cert**: Vercel automatikusan generál Let's Encrypt-ből, amikor a DNS beáll — nem kell semmit csinálnod
- Ellenőrzés: menj a `https://airon.hu`-ra, ha zöld a lakat + betölt a landing page → készen vagy

Ha 30 perc után sem működik, Vercel Settings → Domains oldalon látod a domain státuszát (Valid / Invalid Configuration). Ha invalid, mutat is hibaüzenetet.

---

## SZERKESZTÉS KÉSŐBB

Ha módosítani akarsz a landing page-en (pl. új szöveg, más árazás):

1. Nyisd meg a GitHub-on az `index.html` fájlt
2. `Edit` (ceruza ikon) → módosítsd amit kell
3. `Commit changes` alul
4. **Vercel automatikusan újradeploy-ol** 20-30 másodperc alatt
5. Néhány perc múlva élesben látod a változtatást a `https://airon.hu`-n

Nem kell semmit külön feltölteni. Ez a legerősebb: git push = live deploy.

---

## ANALYTICS (opcionális)

Ha látni akarod hány látogatód van:

**Vercel Web Analytics** (legegyszerűbb, ingyenes):
1. Vercel Dashboard → projekt → `Analytics` tab → `Enable`
2. Kapsz látogató-count, top pages, referrers

**Alternatívák**:
- [Plausible](https://plausible.io) — 9 €/hó, GDPR-safe (nem kell cookie-banner)
- [Umami](https://umami.is) — ingyen self-hosted
- Google Analytics — ingyen, de cookie-banner kell + GDPR-headache

Ha Vercel-lel maradsz és bekapcsolod, `index.html` `<head>` szekcióba tedd:
```html
<script defer src="/_vercel/insights/script.js"></script>
```

---

## SZERKESZTÉSI TIPPEK

**Ha új szekciót akarsz hozzáadni** (pl. Ügyfél-referenciák):
- Nyisd meg `index.html`-t
- Keresd meg pl. a `PILOT` szekció végét (`<!-- PRICING -->` komment előtt)
- Másolj be egy hasonló `<section>` blokkot, cseréld ki a szöveget
- Commit → auto-deploy

**Ha színeket akarsz módosítani**:
- Az `index.html` elején van egy `tailwind.config` script blokk
- Ott vannak a színek (`teal`, `purple`, `bg`, stb.)
- Ha módosítod pl. a `teal` értékét `#00E5B0`-ról `#7DFFDE`-re, minden helyen frissül

**Ha új logót akarsz**:
- Cseréld le a `favicon.svg`-t vagy `airon-logo.svg`-t
- Commit → auto-deploy

---

## PROBLÉMAMEGOLDÁS

**"Not Found" hiba Vercel-en**:
- Ellenőrizd, hogy az `index.html` a repo root-jában van (nem `airon-website/index.html`)
- Ha almappában van, Vercel `Settings` → `General` → `Root Directory`-re állítsd be

**"DNS_PROBE_FINISHED_NXDOMAIN"**:
- Még nem terjedt el a DNS. Várj 5-30 percet.
- Ellenőrizd hogy Porkbun-on tényleg mentetted a record-okat.

**"Your connection is not private" hiba**:
- Az SSL cert még generálódik. 2-5 perc, aztán frissítsd.

**A tartalom megjelenik, de a Tailwind stílusok nem**:
- Ellenőrizd hogy van internet-kapcsolat (Tailwind CDN-ből jön)
- Nyisd meg böngésző DevTools (F12) → `Console` — ha `CDN failed to load` üzenet van, próbáld egy másik CDN-t
- Fallback: mentsd le lokálisan a Tailwind CSS-t

**"AI" szó rosszul néz ki mobilon**:
- A gradient (`<linearGradient id="aiGrad">`) mobil Safari-n néha problémás
- Fallback: a `<tspan fill="url(#aiGrad)">` helyett `<tspan fill="#B083FF">` (solid purple)

---

## MI JÖHET UTÁN

Ha meg akarsz komolyabbat:

1. **Contact form** — hozzáadhatunk [Formspree](https://formspree.io/) vagy [Netlify Forms](https://www.netlify.com/products/forms/) integrációt (5 perc)
2. **Blog-szekció** — átcsapunk Astro-ra vagy Next.js-re, minimális MDX + git-alapú blog
3. **Chat-widget** — Intercom / Crisp / Tawk.to integráció
4. **A/B testing** — Vercel Edge Config vagy Optimizely
5. **Multi-language** — angol verzió (`/en/index.html`)

Bármit szeretnél, csak szólj — 10-30 perc alatt csináljuk.

---

## GYORS-CHECKLIST

- [ ] GitHub account létrehozva
- [ ] `airon-landing` repo létrehozva
- [ ] `index.html` + `airon-logo.svg` + `favicon.svg` feltöltve
- [ ] Vercel account létrehozva (GitHub-bal)
- [ ] Vercel projekt importálva → deploy sikeres
- [ ] Vercel-en `airon.hu` domain hozzáadva
- [ ] Porkbun-on `A record` (`@` → `76.76.21.21`) beállítva
- [ ] Porkbun-on `CNAME` (`www` → `cname.vercel-dns.com`) beállítva
- [ ] 5-30 perc várakozás
- [ ] `https://airon.hu` betöltődik, zöld lakat

Kész! Ha bármi elakad, küldd el a Vercel projekt URL-t + Porkbun DNS screenshot-ot, és megnézem.

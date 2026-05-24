---
name: tallores-website
description: >
  Use this skill to set up a complete restaurant website project from scratch.
  Triggers include: "set up a restaurant website", "create a restaurant site",
  "build a website for my restaurant", "scaffold a restaurant project", or any
  request to generate a restaurant web presence. Also triggers when the user
  wants to generate or regenerate the CLAUDE.md, brand_assets folder, or
  index.html for a restaurant project. Always use this skill before writing
  any restaurant website files — it defines the full architecture, asset
  structure, and generation workflow.
---

# TalloRes Website Skill

This skill scaffolds a complete restaurant website project: it collects restaurant
details, generates the project structure, and produces a first-version website
with Home, Contact, and Reservation sections (plus an optional Menu section if
a PDF is provided). The reservation system uses TalloRes.

---

## Dependencies

This skill relies on two other skills. **Read both before writing any code:**

- `frontend-design` skill — for all visual/CSS/layout decisions
- `tallores-integration` skill — for the TalloRes reservation widget and links

---

## Workflow

### Step 1 — Collect Restaurant Details

Ask the user for the following. Collect all of them before proceeding:

1. **Restaurant name**
2. **Address** (full street address)
3. **Phone number**
4. **Email address**
5. **Type of restaurant** — offer these options:
   - Fine dining
   - Italian
   - Grill / BBQ
   - Asian / Fusion
   - Casual / Bistro
   - Other (ask them to describe)
6. **Visual style** — offer these options:
   - Elegant & dark (moody, luxury)
   - Light & airy (minimalist, fresh)
   - Rustic & warm (earthy, cozy)
   - Bold & modern (high contrast, graphic)

You may ask all questions at once. Do not proceed to Step 2 until all six are answered.

---

### Step 2 — Scaffold the Project Structure

Create the following folder and file structure in the current working directory:

```
./
├── CLAUDE.md                  ← generated in this step
├── serve.mjs                  ← static dev server (see template below)
├── index.html                 ← generated in Step 5 (placeholder for now)
└── brand_assets/
    ├── content.json           ← generated in Step 5
    ├── theme.json             ← generated in Step 5
    └── .gitkeep               ← keeps folder in git
```

#### CLAUDE.md

Generate a `CLAUDE.md` file using the template in `references/CLAUDE.md.template` in this skill,
substituting the restaurant details collected in Step 1.

#### serve.mjs

Use this exact content:

```js
import { createServer } from "http";
import { readFile } from "fs/promises";
import { extname, join } from "path";
import { fileURLToPath } from "url";
import { dirname } from "path";

const __dirname = dirname(fileURLToPath(import.meta.url));
const PORT = 3000;

const MIME = {
  ".html": "text/html",
  ".js": "text/javascript",
  ".css": "text/css",
  ".json": "application/json",
  ".svg": "image/svg+xml",
  ".png": "image/png",
  ".jpg": "image/jpeg",
  ".jpeg": "image/jpeg",
  ".webp": "image/webp",
  ".pdf": "application/pdf",
};

createServer(async (req, res) => {
  let urlPath = req.url === "/" ? "/index.html" : req.url;
  const filePath = join(__dirname, urlPath);
  try {
    const data = await readFile(filePath);
    const ext = extname(filePath);
    res.writeHead(200, { "Content-Type": MIME[ext] || "text/plain" });
    res.end(data);
  } catch {
    res.writeHead(404);
    res.end("Not found");
  }
}).listen(PORT, () => console.log(`Server running at http://localhost:${PORT}`));
```

---

### Step 3 — Ask for Brand Assets

After scaffolding, tell the user:

> "The project structure is ready. Now please add your brand assets to the `brand_assets/` folder:
>
> - **`logo.svg`** or **`logo.png`** — your restaurant logo
> - **One or more photos** — hero image, food shots, interior (`.jpg`, `.png`, or `.webp`)
> - *(Optional)* **`menu.pdf`** — if provided, a Menu section will be added to the site
>
> Let me know when you've added the files, or tell me which ones are already there."

Wait for the user's confirmation before proceeding to Step 4.

---

### Step 4 — Inventory Brand Assets

Once the user confirms, check what's in `brand_assets/`:

```bash
ls brand_assets/
```

Note:
- Whether a logo file exists (`logo.svg` or `logo.png`)
- Names of any image files (`.jpg`, `.png`, `.webp`) — use the first one as hero, others as section images
- Whether `menu.pdf` exists → if yes, the Menu section will be generated

---

### Step 5 — Generate the Website Files

Read the `frontend-design` skill and `tallores-integration` skill now if not already done.

Then generate three files:

#### A. `brand_assets/theme.json`

Generate design tokens appropriate for the restaurant type and visual style chosen in Step 1.
See `references/theme-presets.md` in this skill for token sets per visual style.

All colors, fonts, shadows, spacing must be defined here. No hardcoded values anywhere else.

#### B. `brand_assets/content.json`

Populate all text content for the site. Structure:

```json
{
  "meta": { "title": "", "description": "" },
  "restaurant": { "name": "", "tagline": "", "hero_subtitle": "", "story": "" },
  "assets": {
    "logo": "brand_assets/logo.svg",
    "hero_image": "brand_assets/<filename>",
    "images": []
  },
  "nav": {
    "links": [
      { "label": "Home", "href": "#home" },
      { "label": "Menu", "href": "#menu" },
      { "label": "Contact", "href": "#contact" }
    ],
    "cta": { "label": "Reserve a Table", "href": "#reservation" }
  },
  "menu_pdf": "brand_assets/menu.pdf",
  "contact": {
    "address": "",
    "phone": "",
    "email": "",
    "hours": []
  },
  "footer": {
    "address": "",
    "copyright": ""
  }
}
```

Omit `menu_pdf` if no `menu.pdf` was found.

Write natural, on-brand placeholder copy for `tagline`, `hero_subtitle`, and `story` based on the restaurant type.

#### C. `index.html`

Generate a single-page site that:

1. Loads `brand_assets/theme.json` and `brand_assets/content.json` via `fetch()` on startup
2. Calls `initTheme(theme)` to write all tokens as CSS custom properties on `:root`
3. Calls `renderPage(content)` to populate all DOM nodes from content
4. Uses **only** `var(--color-*)`, `var(--font-heading)`, `var(--font-body)` etc. — zero hardcoded hex or font names
5. Includes these sections:
   - **`#home`** — Hero with full-bleed image, restaurant name, tagline, CTA button
   - **`#menu`** — Only if `menu.pdf` was found: PDF embed or download link
   - **`#contact`** — Address, phone, email, opening hours, Google Maps embed placeholder
   - **`#reservation`** — TalloRes iframe embed (see TalloRes integration below)
6. Includes a sticky nav with smooth-scroll links
7. Includes a footer

Follow all anti-generic guardrails from the `frontend-design` skill.

---

### Step 6 — TalloRes Reservation Integration

Follow the `tallores-integration` skill for the reservation section:

1. Ask the user for their **TalloRes RestaurantId**
2. Store it as a `<meta name="tallores-restaurant-id">` tag in `<head>`
3. Add the **floating widget** script before `</body>`
4. Embed the **reservation iframe** in the `#reservation` section:
   ```html
   <iframe
     src="https://widget.tallores.com/form/{{restaurantId}}"
     width="100%"
     height="600px"
     style="border:none; overflow:hidden;"
     loading="lazy">
   </iframe>
   ```
5. Wire the nav CTA button href to `https://widget.tallores.com/{{restaurantId}}`

---

### Step 7 — Final Checklist

Before handing off, verify:

- [ ] `CLAUDE.md` exists and contains correct restaurant details
- [ ] `serve.mjs` exists
- [ ] `brand_assets/theme.json` has all color, font, shadow tokens
- [ ] `brand_assets/content.json` has all text content and correct asset paths
- [ ] `index.html` uses only CSS custom properties (no hardcoded hex/fonts)
- [ ] TalloRes RestaurantId is stored and used consistently
- [ ] Floating widget script is before `</body>`
- [ ] Reservation iframe is in `#reservation`
- [ ] Menu section present **only if** `menu.pdf` exists

Then tell the user:

> "Your restaurant site is ready. Start the dev server with:
> ```
> node serve.mjs
> ```
> Then open http://localhost:3000 to preview it.
>
> To update content or styles, edit `brand_assets/content.json` and `brand_assets/theme.json`.
> To swap the brand entirely, replace the `brand_assets/` folder."

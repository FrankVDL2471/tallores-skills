---
name: tallores-integration
description: >
  Use this skill whenever the user wants to integrate the TalloRes reservation system into a website.
  Triggers include any mention of "TalloRes", "reservation system", "reservation widget", "booking widget",
  "online reservations", or requests to add a reservation feature, booking button, or booking page to a website.
  Also use when the user asks to update or modify an existing TalloRes integration.
  Always use this skill before modifying any website files related to reservations or TalloRes.
---

# TalloRes Integration Skill

TalloRes is an online reservation system for restaurants. There are two integration methods:
1. **Floating widget** — A script embedded before `</body>` on each page, showing a floating reservation button.
2. **Full page** — A redirect or iframe used when there's a reservation link or navigation item.

---

## Step 1: Get the RestaurantId

Before making any changes, ask the user for their **RestaurantId**.

> "To integrate TalloRes, I'll need your RestaurantId. You can find it on your TalloRes settings page at https://app.tallores.com/nl/settings/reservation-widget — it's shown in the widget embed code. Could you share it with me?"

Do not proceed until the RestaurantId is provided.

---

## Step 2: Store the RestaurantId in the website configuration

Once you have the RestaurantId, store it in the website's configuration so it's reused consistently. How to do this depends on the website's tech stack:

- **Static HTML site**: Add a comment or a `<meta>` tag like `<meta name="tallores-restaurant-id" content="{{restaurantId}}">` in the `<head>` of a shared layout/template, or note it prominently in a config file.
- **CMS / config file** (e.g. `config.js`, `_config.yml`, `.env`, `settings.json`): Add a key like `TALLORES_RESTAURANT_ID={{restaurantId}}` or `talloresRestaurantId: "{{restaurantId}}"`.
- **React / Next.js / Vue / etc.**: Add to the relevant config or environment file (e.g. `.env.local`: `NEXT_PUBLIC_TALLORES_RESTAURANT_ID={{restaurantId}}`).

Adapt the storage approach to whatever convention the project already uses. Always confirm with the user where it was stored.

---

## Step 3: Integrate the Floating Widget

Add the following script **directly before the closing `</body>` tag** on every page (or in the shared layout/template file):

```html
<script src="https://widget.tallores.com/widget.min.js?restaurantId={{restaurantId}}"></script>
```

Replace `{{restaurantId}}` with the actual RestaurantId.

**Where to add it:**
- Static HTML: In each `.html` file, or in a shared footer/layout partial.
- Template engines (Twig, Blade, Liquid, etc.): In the base layout template.
- React/Next.js: In `_document.js` or equivalent.
- WordPress: In `footer.php` of the active theme, or via a footer hook.

---

## Step 4: Integrate the Full Page Reservation (if applicable)

If the website has a **reservation link or navigation item** (e.g. a "Reserve a table" button or nav link), update it to use the TalloRes full-page URL.

### Option A — Direct redirect (preferred for links/buttons)

Update the `href` of the reservation link to:

```
https://widget.tallores.com/{{restaurantId}}
```

Example:
```html
<a href="https://widget.tallores.com/{{restaurantId}}">Reserve a table</a>
```

### Option B — Embedded iframe (for a dedicated reservations page)

If the site has a dedicated reservations page (e.g. `/reservations`), embed the form using an iframe:

```html
<iframe
  src="https://widget.tallores.com/form/{{restaurantId}}"
  width="100%"
  height="600px"
  style="border:none; overflow:hidden;"
  loading="lazy">
</iframe>
```

Use the iframe when the user wants the reservation form to appear inline on a page rather than redirecting away.

---

## Decision Guide: Which integration(s) to apply?

| Situation | Apply |
|---|---|
| Every page should show a floating reservation button | Floating widget (Step 3) |
| There's a "Reserve" link/button in the navigation or on the page | Full page redirect — Option A (Step 4) |
| There's a dedicated `/reservations` or `/book` page | Full page iframe — Option B (Step 4) |
| All of the above | All three |

When unsure, ask the user: "Does your site have a dedicated reservations page or a reservation link in the navigation? That determines which integration method to use in addition to the floating widget."

---

## Reminders

- Always use the **same RestaurantId** stored in Step 2 for every integration point.
- The floating widget script must come **directly before `</body>`**, not in `<head>`.
- For the iframe, `width="100%"` and `height="600px"` are sensible defaults — suggest the user adjust height to fit their layout.
- After making changes, suggest the user preview the site to confirm the widget appears and reservation links work correctly.

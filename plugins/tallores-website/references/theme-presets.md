# Theme Presets

Use the token set matching the visual style chosen by the user in Step 1.
These are starting points — adjust tones to suit the restaurant type.

---

## Elegant & Dark (moody, luxury)

```json
{
  "colors": {
    "base": "#0F0E0D",
    "surface": "#1A1815",
    "card": "#231F1C",
    "elevated": "#2C2723",
    "primary": "#C9A96E",
    "primary-hover": "#DFC08A",
    "text-heading": "#F5F0E8",
    "text-body": "#C4BDB2",
    "text-muted": "#7A736A",
    "border": "#3A3530",
    "overlay": "rgba(15,14,13,0.7)"
  },
  "fonts": {
    "heading": "'Playfair Display', Georgia, serif",
    "body": "'Manrope', system-ui, sans-serif"
  },
  "shadows": {
    "card": "0 2px 8px rgba(0,0,0,0.4), 0 8px 32px rgba(0,0,0,0.3)",
    "elevated": "0 4px 16px rgba(0,0,0,0.5), 0 16px 48px rgba(0,0,0,0.4)",
    "glow": "0 0 24px rgba(201,169,110,0.15)"
  },
  "spacing": {
    "section": "6rem",
    "card-padding": "2rem",
    "nav-height": "4.5rem"
  },
  "radius": {
    "card": "0.5rem",
    "button": "0.25rem",
    "input": "0.25rem"
  }
}
```

Google Fonts import: `Playfair+Display:wght@400;500;700&family=Manrope:wght@300;400;500;600`

---

## Light & Airy (minimalist, fresh)

```json
{
  "colors": {
    "base": "#FAFAF8",
    "surface": "#F4F3EF",
    "card": "#FFFFFF",
    "elevated": "#FFFFFF",
    "primary": "#2D6A4F",
    "primary-hover": "#1B4332",
    "text-heading": "#1A1A18",
    "text-body": "#4A4A46",
    "text-muted": "#9A9A94",
    "border": "#E8E7E2",
    "overlay": "rgba(250,250,248,0.85)"
  },
  "fonts": {
    "heading": "'Cormorant Garamond', Georgia, serif",
    "body": "'DM Sans', system-ui, sans-serif"
  },
  "shadows": {
    "card": "0 1px 4px rgba(0,0,0,0.06), 0 4px 16px rgba(0,0,0,0.05)",
    "elevated": "0 2px 8px rgba(0,0,0,0.08), 0 8px 32px rgba(0,0,0,0.06)",
    "glow": "0 0 20px rgba(45,106,79,0.08)"
  },
  "spacing": {
    "section": "7rem",
    "card-padding": "2.5rem",
    "nav-height": "5rem"
  },
  "radius": {
    "card": "1rem",
    "button": "2rem",
    "input": "0.5rem"
  }
}
```

Google Fonts import: `Cormorant+Garamond:wght@300;400;500;600&family=DM+Sans:wght@300;400;500`

---

## Rustic & Warm (earthy, cozy)

```json
{
  "colors": {
    "base": "#F2EDE4",
    "surface": "#EBE4D8",
    "card": "#FAF7F2",
    "elevated": "#FFFFFF",
    "primary": "#8B4513",
    "primary-hover": "#6B3410",
    "text-heading": "#2C1F14",
    "text-body": "#5C4A38",
    "text-muted": "#9C8470",
    "border": "#D4C4B0",
    "overlay": "rgba(44,31,20,0.6)"
  },
  "fonts": {
    "heading": "'Lora', Georgia, serif",
    "body": "'Source Sans 3', system-ui, sans-serif"
  },
  "shadows": {
    "card": "0 2px 6px rgba(139,69,19,0.08), 0 6px 24px rgba(44,31,20,0.08)",
    "elevated": "0 4px 12px rgba(139,69,19,0.12), 0 12px 40px rgba(44,31,20,0.1)",
    "glow": "0 0 20px rgba(139,69,19,0.12)"
  },
  "spacing": {
    "section": "5.5rem",
    "card-padding": "2rem",
    "nav-height": "4.5rem"
  },
  "radius": {
    "card": "0.75rem",
    "button": "0.375rem",
    "input": "0.375rem"
  }
}
```

Google Fonts import: `Lora:wght@400;500;600;700&family=Source+Sans+3:wght@300;400;600`

---

## Bold & Modern (high contrast, graphic)

```json
{
  "colors": {
    "base": "#F8F8F6",
    "surface": "#F0F0EE",
    "card": "#FFFFFF",
    "elevated": "#FFFFFF",
    "primary": "#E63224",
    "primary-hover": "#C52A1E",
    "text-heading": "#0A0A0A",
    "text-body": "#2A2A2A",
    "text-muted": "#7A7A7A",
    "border": "#E0E0DE",
    "overlay": "rgba(10,10,10,0.65)"
  },
  "fonts": {
    "heading": "'Bebas Neue', Impact, sans-serif",
    "body": "'Inter', system-ui, sans-serif"
  },
  "shadows": {
    "card": "0 2px 4px rgba(0,0,0,0.08), 4px 4px 0 #0A0A0A",
    "elevated": "0 4px 8px rgba(0,0,0,0.12), 6px 6px 0 #0A0A0A",
    "glow": "0 0 24px rgba(230,50,36,0.2)"
  },
  "spacing": {
    "section": "6rem",
    "card-padding": "2rem",
    "nav-height": "4rem"
  },
  "radius": {
    "card": "0",
    "button": "0",
    "input": "0"
  }
}
```

Google Fonts import: `Bebas+Neue&family=Inter:wght@300;400;500;600`

---

## Adapting Tokens

- For **fine dining**, lean toward Elegant & Dark or Light & Airy
- For **grill / BBQ**, Rustic & Warm works best
- For **Italian**, Rustic & Warm or Light & Airy
- For **Asian / Fusion**, Bold & Modern or Elegant & Dark
- For **casual bistro**, Light & Airy or Rustic & Warm

Always add the Google Fonts `<link>` tag in `<head>` matching the preset used.

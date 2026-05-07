# Luminous Website — Implementation Plan

## Brand Context
Luminous is an Indonesian local fashion brand focused on oversized/streetwear casual clothing for urban youth. They're launching a new product: **Baggy Sweatpants**. This website serves as company profile + product catalog.

## Tech Stack
- Raw HTML5 only (4 separate `.html` files)
- Tailwind CSS via CDN
- Vanilla JS (inline, no libraries)
- Google Fonts via CDN

## Color Palette & Typography
```
--cream: #FBF7F4  (page background)
--sand:  #E5DED2  (card/surface bg)
--brown: #685D54  (accent, secondary text)
--dark:  #232323  (primary text, headings)
```
Fonts: `Playfair Display` (headings/display) + `DM Sans` (body/UI)

Add to every page:
```html
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700;900&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<script src="https://cdn.tailwindcss.com"></script>
<script>
  tailwind.config = {
    theme: { extend: {
      colors: { cream:'#FBF7F4', sand:'#E5DED2', brown:'#685D54', dark:'#232323' },
      fontFamily: { display:['"Playfair Display"','serif'], body:['"DM Sans"','sans-serif'] }
    }}
  }
</script>
```

## Shared Animations (add to every page `<style>`)
```css
@keyframes fadeUp { from { opacity:0; transform:translateY(24px); } to { opacity:1; transform:translateY(0); } }
@keyframes marquee { from { transform:translateX(0); } to { transform:translateX(-50%); } }
.anim-1 { animation: fadeUp 0.7s ease forwards 0.2s; opacity:0; }
.anim-2 { animation: fadeUp 0.7s ease forwards 0.5s; opacity:0; }
.anim-3 { animation: fadeUp 0.7s ease forwards 0.8s; opacity:0; }
.scroll-reveal { opacity:0; transform:translateY(20px); transition: opacity 0.6s ease, transform 0.6s ease; }
.scroll-reveal.visible { opacity:1; transform:translateY(0); }
```
Scroll observer (add to every page before `</body>`):
```js
const obs = new IntersectionObserver(e => e.forEach(x => x.isIntersecting && x.target.classList.add('visible')), {threshold:0.15});
document.querySelectorAll('.scroll-reveal').forEach(el => obs.observe(el));
```

## Shared Navbar (copy to all pages)
Fixed top bar. Logo left (`Playfair Display`, bold), nav links right (`DM Sans`, uppercase, tracking-widest). Transparent → bg-cream on scroll. Active page link has brown underline.
```js
window.addEventListener('scroll', () => {
  document.getElementById('navbar').classList.toggle('bg-cream shadow-sm', window.scrollY > 10);
});
```

## Shared Footer
Dark bg (`#232323`), sand text. Brand name left, nav links right.

---

## Pages

### 1. `index.html` — Landing Page
Single full-viewport hero. No navbar. Minimal top bar with logo + "Enter →" link.

Content:
- Giant brand name: **LUMINOUS** (`Playfair Display`, 10–14rem, dark)
- Subtitle: *Local Streetwear Since 2020*
- CTA button: `Enter the Collection →` → links to `home.html`
- Bottom tagline: *"Oversized. Minimal. Local."*

Animations: brand name `.anim-1`, subtitle `.anim-2`, CTA `.anim-3`. CTA hover: arrow shifts right.

---

### 2. `home.html` — Home Page
Navbar + footer. 4 sections:

**Section 1 — Hero**
Full-height. Large heading: *"NEW DROP — Baggy Sweatpants AW 2025"*. Two CTA buttons: `Shop Now → catalog.html` and `Our Story → latar-belakang.html`. Heading uses `.anim-1`, buttons `.anim-2`.

**Section 2 — Brand Intro (2-col)**
Left: decorative block (bg-sand, tall rectangle). Right: heading "About Luminous" + short paragraph about the brand being minimalist, comfortable, local. Link to latar-belakang.html. Add `.scroll-reveal`.

**Section 3 — Product Teaser (3 cards)**
Show 3 product cards (preview from catalog). Each card: color swatch block (use brand colors as product color), product name, price. Below cards: `View Full Catalog →` button. Cards use `.scroll-reveal`.

**Section 4 — Marquee Strip**
Full-width dark bg, cream text. Repeating: `MINIMALIST · COMFORTABLE · LOCAL · OVERSIZED ·`. Animated with `marquee` keyframe, infinite loop.

---

### 3. `latar-belakang.html` — Background/Story Page
Navbar + footer. Editorial article layout.

Page header: large title "LATAR BELAKANG", subtitle "The Story Behind Luminous".

3 content sections, each with section number (brown, `DM Sans`), heading (`Playfair Display`), and 2–3 paragraphs. Sections use `.scroll-reveal`.

Content:
1. **Tentang Luminous** — Brand was founded to serve Indonesian urban youth with oversized, minimalist, comfortable streetwear casual clothing. Built strong brand identity through design and material quality.
2. **Tantangan Industri** — Local fashion industry is increasingly competitive. Luminous needs innovation to stay relevant and maintain appeal to young consumers.
3. **Inovasi: Baggy Sweatpants** — After internal discussion and expert input, Luminous decided to develop baggy sweatpants as a new product line. This expands their product range and strengthens their position as a brand adaptive to youth lifestyle trends.

On `md:` screens: 2-col layout — left col is sticky quote (*"Oversized bukan sekedar ukuran, tapi sebuah gaya hidup."*), right col is article body.

---

### 4. `catalog.html` — Product Catalog
Navbar + footer.

**Header**: "THE COLLECTION" + "AW 2025 — Baggy Sweatpants"

**Filter bar**: buttons for `All`, `Sweatpants`, `Oversized Tees`, `Hoodies`. Active = `bg-dark text-cream`, inactive = `border-brown text-brown`. JS toggles visibility of product cards by `data-category` attribute.

**Product Grid**: 3-col desktop, 2-col tablet, 1-col mobile. Cards stagger-fade on load.

**Product data** (use CSS color blocks as product images — no real images needed):

| Name | Category | Price | Swatch Color |
|---|---|---|---|
| Baggy Sweatpants — Ash | Sweatpants | Rp 349.000 | #E5DED2 |
| Baggy Sweatpants — Onyx | Sweatpants | Rp 349.000 | #232323 |
| Baggy Sweatpants — Mocha | Sweatpants | Rp 349.000 | #685D54 |
| Oversized Tee — Blanc | Oversized Tees | Rp 199.000 | #FBF7F4 |
| Oversized Tee — Charcoal | Oversized Tees | Rp 199.000 | #232323 |
| Hoodie — Stone | Hoodies | Rp 449.000 | #E5DED2 |

**Product card structure**:
```
[color swatch block, aspect-square]
Product Name     (Playfair Display)
Category         (DM Sans, brown, small)
Rp XXX.000       (DM Sans, dark)
[Detail] button
```

Card hover: `translateY(-4px)` + soft shadow. On "Detail" click → modal overlay with product name, description, sizes (S/M/L/XL/XXL), and "Hubungi Kami" button.

Filter JS:
```js
document.querySelectorAll('.filter-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    const cat = btn.dataset.filter;
    document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    document.querySelectorAll('.product-card').forEach(card => {
      card.style.display = (cat === 'all' || card.dataset.category === cat) ? 'block' : 'none';
    });
  });
});
```

---

## File Structure
```
luminous/
├── index.html
├── home.html
├── latar-belakang.html
└── catalog.html
```

## Build Order
1. `index.html` (landing)
2. `home.html` (home)
3. `latar-belakang.html` (story)
4. `catalog.html` (catalog)

Set `lang="id"` on all `<html>` tags. Files must open directly in browser — no build step required.

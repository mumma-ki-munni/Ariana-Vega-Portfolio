# Ariana Vega — Architecture & Interior Architecture Studio

A sleek, editorial single-page portfolio designed for high-end architects, spatial designers, and interior studios. Built with vanilla HTML5, Tailwind CSS, and lightweight JavaScript, the site combines contemporary brutalist geometry with warm, tactile architectural sensibilities (obsidian, alabaster, travertine, and champagne bronze accents).

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Design System & Aesthetics](#design-system--aesthetics)
- [Technology Stack](#technology-stack)
- [File Structure](#file-structure)
- [Getting Started](#getting-started)
- [Customization Guide](#customization-guide)
  - [1. Modifying Color Tokens & Typography](#1-modifying-color-tokens--typography)
  - [2. Adding or Editing Portfolio Projects](#2-adding-or-editing-portfolio-projects)
  - [3. Connecting the Inquiry Form to a Backend](#3-connecting-the-inquiry-form-to-a-backend)
- [Browser Compatibility & Performance](#browser-compatibility--performance)
- [License & Credits](#license--credits)

---

## Overview

The studio website presents a curated portfolio for **Ariana Vega**, focusing on monolithic residential architecture, heritage restorations, and bespoke collectible furniture design across Europe and North America. It is structured entirely within a zero-dependency, single-file architecture to ensure immediate deployment and maximum runtime speed.

---

## Key Features

- **Editorial Arched Frames:** Custom CSS arched geometry mimicking classical arcade silhouettes and editorial monographs.
- **Glassmorphism Header & Drawer:** Fixed backdrop-blur navigation bar with a responsive sliding mobile menu.
- **Interactive Portfolio Filtering:** Real-time client-side category filtering (`All`, `Residential`, `Hospitality`, `Cultural`) without page reloads.
- **Dynamic Project Lightbox Modal:** Full-screen modal case study previewing high-resolution project photography, project specifications, and commission prompts.
- **Scroll-Triggered Animated Counters:** Smooth numerical metric counter (`IntersectionObserver`) for studio statistics.
- **Interactive Consultation Form:** Bespoke commission intake form with custom budget pill selectors and integrated toast alert feedback (eliminating disruptive browser alerts).
- **Clipboard Utility:** One-click studio email copy feature with transient toast feedback.
- **Accessibility & UX:** Keyboard navigation (Escape key support for modal dismissal), smooth scrolling (`scroll-smooth`), and contrast ratios.

---

## Design System & Aesthetics

| Token | Hex Code | Purpose |
| :--- | :--- | :--- |
| **Obsidian** | `#0E0E0D` | Primary deep-space background |
| **Charcoal** | `#171615` | Surface cards & structural containers |
| **Travertine** | `#ECE7DD` | Light neutral accents & borders |
| **Alabaster** | `#F7F5F0` | Primary display & heading typography |
| **Sandstone** | `#DDD6C7` | Muted secondary body copy |
| **Bronze** | `#B59473` | Primary accent, indicators, and buttons |
| **Deep Bronze**| `#8C6C4C` | Hover states and warm transitions |

### Typography
- **Headings & Brand:** [`Syne`](https://fonts.google.com/specimen/Syne) (Weights: 600, 700, 800)
- **Body & Captions:** [`Plus Jakarta Sans`](https://fonts.google.com/specimen/Plus+Jakarta+Sans) (Weights: 300, 400, 500, 600)

---

## Technology Stack

- **HTML5:** Semantic structural layout (`<header>`, `<main>`, `<section>`, `<article>`, `<dialog>/modal`).
- **Tailwind CSS (CDN):** Utility-first styling with an inline configuration object.
- **Vanilla JavaScript (ES6+):**
  - DOM event listeners for mobile navigation and category filters.
  - `IntersectionObserver` API for viewport-triggered statistics counters.
  - Native Clipboard API for asynchronous copy-to-clipboard actions.
- **Google Fonts:** Asynchronous font pre-connects and stylesheet integration.

---

## File Structure

```text
ariana-vega-portfolio/
├── index.html        # Complete standalone website (HTML, styles, scripts)
└── README.md         # Documentation & setup instructions
```

---

## Getting Started

Because the project is self-contained with no build tooling required, you can launch it using any static server or open it directly in a web browser.

### Option 1: Direct File Opening
Double-click `index.html` or drag it into any modern desktop browser (Chrome, Firefox, Safari, Edge).

### Option 2: Local HTTP Server (Recommended)
Running through an HTTP server ensures external image links and modern browser security permissions execute properly:

Using **Node.js**:
```bash
# Using npx and serve
npx serve .

# Or using http-server
npx http-server .
```

Using **Python 3**:
```bash
python3 -m http.server 8000
```
Then navigate to `http://localhost:8000` in your browser.

Using **VS Code Live Server**:
Right-click `index.html` inside VS Code and select **"Open with Live Server"**.

---

## Customization Guide

### 1. Modifying Color Tokens & Typography
In the `<head>` of `index.html`, locate the `tailwind.config` script block to alter brand colors or font pairings:

```javascript
tailwind.config = {
  theme: {
    extend: {
      fontFamily: {
        display: ['Syne', 'sans-serif'],
        sans: ['Plus Jakarta Sans', 'sans-serif'],
      },
      colors: {
        alabaster: '#F7F5F0',
        travertine: '#ECE7DD',
        sandstone: '#DDD6C7',
        bronze: '#B59473',
        deepbronze: '#8C6C4C',
        charcoal: '#171615',
        obsidian: '#0E0E0D',
      }
    }
  }
}
```

### 2. Adding or Editing Portfolio Projects
Each project card inside `#projectsGrid` has a simple markup signature:

```html
<article 
  class="project-item group rounded-3xl overflow-hidden glass-card cursor-pointer" 
  data-category="residential" 
  onclick="openLightbox(
    'Project Name', 
    'City, Country', 
    '2025', 
    'Residential', 
    'Project narrative and structural summary...', 
    'https://images.unsplash.com/...'
  )">
  <!-- Thumbnail & Card Content -->
</article>
```
To register a new project:
1. Set the `data-category` attribute (`residential`, `hospitality`, or `cultural`).
2. Pass the desired metadata parameters directly into the `openLightbox(...)` click handler.

### 3. Connecting the Inquiry Form to a Backend
The inquiry form handles submissions inside `<script>` via the `inquiryForm.addEventListener('submit', ...)` function. To connect this to a service such as [Formspree](https://formspree.io/), [Resend](https://resend.com/), or an AWS Lambda endpoint:

```javascript
inquiryForm.addEventListener('submit', async (e) => {
  e.preventDefault();
  
  const payload = {
    name: document.getElementById('userName').value,
    email: document.getElementById('userEmail').value,
    scope: document.getElementById('projectScope').value,
    location: document.getElementById('projectLocation').value,
    budget: document.querySelector('input[name="budget"]:checked').value,
    notes: document.getElementById('projectNotes').value
  };

  try {
    const res = await fetch('https://your-api-endpoint.com/inquire', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(payload)
    });

    if (res.ok) {
      showToast('Inquiry Received', 'Our partners will review your briefing within 48 hours.');
      inquiryForm.reset();
    }
  } catch (error) {
    showToast('Transmission Error', 'Unable to deliver message. Please email directly.', false);
  }
});
```

---

## Browser Compatibility & Performance

- **Chrome / Chromium:** Version 90+ (Full support)
- **Safari:** Version 15+ (Full support for `-webkit-backdrop-filter` and flex gaps)
- **Firefox:** Version 88+ (Full support)
- **Mobile Browsers:** iOS Safari & Chrome for Android fully supported with responsive touch states.

---

## License & Credits

- **Fonts:** [Syne](https://fonts.google.com/specimen/Syne) by Lucas Descroix & [Plus Jakarta Sans](https://fonts.google.com/specimen/Plus+Jakarta+Sans) by Tokotype (Open Font License).
- **Stock Imagery:** Unsplash architectural photography.
- **License:** Distributed under the [MIT License](https://opensource.org/licenses/MIT). Free for personal and commercial adaptation.
<div align="center">

# NEXA — Phone Sale HomePage

**A dark, neon-accented product landing page with an animated preloader — built with HTML, Tailwind CSS & Vanilla JavaScript.**

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Font Awesome](https://img.shields.io/badge/Font_Awesome-538DD7?style=for-the-badge&logo=fontawesome&logoColor=white)

![Repo size](https://img.shields.io/github/repo-size/student-BPSMV/Phone-Sale-HomePage?style=flat-square&color=39ff5a)
![Last commit](https://img.shields.io/github/last-commit/student-BPSMV/Phone-Sale-HomePage?style=flat-square&color=39ff5a)
![Stars](https://img.shields.io/github/stars/student-BPSMV/Phone-Sale-HomePage?style=flat-square&color=39ff5a)

</div>

---

## Preview

<div align="center">

### Loading Screen
<img src="images/loading.png" alt="NEXA loading screen with animated spinner and progress bar" width="100%">

<br><br>

### Hero Section
<img src="images/full_View.png" alt="NEXA hero section with product details and pricing" width="100%">

</div>

---

## Overview

The page loads in two stages. A full-screen preloader animates for three seconds — a spinner, a pulsing label, and a sliding progress bar — and then the navbar and hero section take over. That handoff is driven entirely by a single `setTimeout` call.

---

## Features

- Full-screen preloader with animated spinner and progress bar
- Navbar with brand logo, links, and a pill-shaped CTA button
- Hero section with headline, feature highlights, price, and action buttons
- Neon green (`#39ff5a`) accent on a near-black palette
- Icon set via Font Awesome, typography via Inter
- Styled with the Tailwind CLI (`input.css` compiles to `output.css`)

---

## Tech Stack

| Technology | Role |
| :--- | :--- |
| **HTML5** | Page structure and semantic markup |
| **Tailwind CSS** | Utility-first styling and custom keyframe animations |
| **JavaScript (ES6)** | Preloader timing and DOM control |
| **Font Awesome** | Feature and button icons |
| **Google Fonts** | Inter typeface |

---

## ⏱️ The Role of `setTimeout` in This Project

`setTimeout` is the one piece of JavaScript that makes the whole loading experience work. Remove it and the preloader either never appears or never leaves.

### The code

```js
let loadingPage = document.querySelector("#loading");
let heroPage    = document.querySelector("#herosection");
let navbar      = document.querySelector("#navbar");

// Hide the real content the moment the script runs
heroPage.style.display = "none";
navbar.style.display   = "none";

function showScreen() {
  loadingPage.style.display = "none";   // hide the loader
  heroPage.style.display    = "block";  // reveal the hero section
  navbar.style.display      = "block";  // reveal the navbar
}

// Run showScreen() once, 3 seconds after load
setTimeout(showScreen, 3000);
```

### The flow

```
Page loads
   │
   ├─ #loading          →  visible
   ├─ #navbar           →  hidden
   └─ #herosection      →  hidden
   │
   ▼  setTimeout waits 3000ms (spinner + progress bar animate)
   │
showScreen() fires
   │
   ├─ #loading          →  hidden
   ├─ #navbar           →  visible
   └─ #herosection      →  visible
```

<details>
<summary><b>1. It creates the delay that makes the preloader visible</b></summary>

<br>

JavaScript executes top to bottom almost instantly. Calling `showScreen()` directly would hide the loader in the same frame it appeared, so the user would never see it. `setTimeout` schedules the function to run *later*, giving the spinner and progress bar a real 3-second window to play.

</details>

<details>
<summary><b>2. It's asynchronous, so the page never freezes</b></summary>

<br>

`setTimeout` hands the callback to the browser's Web API and lets the main thread continue. During those 3 seconds the CSS animations (`animate-spin`, `animate-progressBar`, `animate-pulse`) keep running smoothly.

A blocking delay would lock the UI entirely and freeze the spinner mid-rotation:

```js
// ❌ Never do this — blocks the main thread
const start = Date.now();
while (Date.now() - start < 3000) {}
```

</details>

<details>
<summary><b>3. It keeps JavaScript in sync with the CSS animations</b></summary>

<br>

The progress bar and spinner are pure CSS. The `3000` in `setTimeout` is what keeps the JavaScript aligned with them — change the animation duration and you'd change this number too, otherwise the bar gets cut off partway through.

</details>

<details>
<summary><b>4. How <code>setTimeout</code> works in general</b></summary>

<br>

```js
setTimeout(callbackFunction, delayInMilliseconds);
```

- Runs the callback **once** after the delay — use `setInterval` for repeating behaviour.
- The delay is a **minimum**, not a guarantee. The callback only fires once the call stack is clear.
- Returns a numeric ID you can cancel with `clearTimeout()`:

```js
const timerId = setTimeout(showScreen, 3000);
clearTimeout(timerId); // cancels it before it runs
```

</details>

<details>
<summary><b>5. A note on real-world usage</b></summary>

<br>

The 3 seconds here is a fixed, deliberate delay — good for a branded intro or a demo. Production sites usually hide the loader once assets have genuinely finished loading:

```js
window.addEventListener("load", showScreen);
```

`setTimeout` is often kept alongside it as a safety net, so the loader can never get stuck if an asset fails:

```js
window.addEventListener("load", showScreen);
setTimeout(showScreen, 5000); // fallback
```

</details>

---

## Project Structure

```
Phone-Sale-HomePage/
├── images/
│   ├── full_View.png      # screenshot — hero section
│   ├── loading.png        # screenshot — loading screen
│   └── phone.png          # product image
├── src/
├── index.html             # main page
├── input.css              # Tailwind source
├── output.css             # compiled stylesheet
├── package.json
└── package-lock.json
```

---

## Getting Started

**1. Clone the repository**

```bash
git clone https://github.com/student-BPSMV/Phone-Sale-HomePage.git
cd Phone-Sale-HomePage
```

**2. Install dependencies**

```bash
npm install
```

**3. Build the CSS**

```bash
npx tailwindcss -i ./input.css -o ./output.css --watch
```

**4. Open the page**

Open `index.html` in your browser, or use the Live Server extension in VS Code.

---

## What I Learned

- Scheduling code with `setTimeout` to control page flow
- DOM manipulation using `querySelector` and `style.display`
- Why asynchronous JavaScript keeps the UI responsive
- Building a consistent design system with Tailwind utilities
- Laying out responsive sections with Flexbox

---

## Possible Improvements

- [ ] Fade the loader out with a CSS transition instead of an instant `display` switch
- [ ] Swap the fixed timer for the `load` event, keeping `setTimeout` as a fallback
- [ ] Make the hero section fully responsive on mobile
- [ ] Add working cart and wishlist interactions
- [ ] Extract the JavaScript into its own `script.js` file

---

<div align="center">

**Built by [student-BPSMV](https://github.com/student-BPSMV)**

If this helped you, consider giving it a ⭐

</div>

# Lô Lô Chải Interactive Room — Demo Build Spec
> Target: Clickable prototype demo only. No backend, no CMS, no routing. Single HTML file or Vite vanilla JS project.

---

## 1. Project Overview

**Concept:** An interactive 2D room scene inspired by a traditional Lô Lô Chải ethnic house in Hà Giang, Vietnam. Users explore the room by clicking on 9 cultural artifact hotspots. Each click reveals a detail panel with a real photo and description text.

**Reference:** l2kroom.net — same interaction paradigm: static illustrated scene + clickable hotspots + sliding info panel.

**Scope (demo only):**
- ✅ Static scene image (user-provided)
- ✅ 9 clickable hotspots with hover + click animations
- ✅ Info panel slide-up with photo + text per artifact
- ✅ Sound toggle button (optional, can be stubbed)
- ✅ Object counter (X / 9 discovered)
- ❌ No backend, no database, no CMS
- ❌ No mobile optimization required for demo

---

## 2. Tech Stack

```
HTML5 + CSS3 + Vanilla JavaScript   → No framework needed for demo
GSAP 3 (CDN)                        → All animations (bounce, glow, panel slide)
Howler.js (CDN, optional)           → Sound effects (can stub with empty functions)
Vite (optional)                     → Only if multi-file structure needed
```

Single-file delivery preferred: `index.html` with embedded `<style>` and `<script>`.

CDN imports:
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/howler/2.2.3/howler.min.js"></script>
```

---

## 3. File Structure

```
/project
  index.html              ← main entry, all logic here
  /assets
    /scene
      room-bg.png         ← full isometric/2D room scene (user provided)
    /artifacts
      01-bep-lua.png      ← fireplace (real photo, 1:1 ratio)
      02-noi-dong.png     ← bronze pot
      03-gui-tre.png      ← bamboo basket
      04-trong-dong.png   ← bronze drum
      05-vay-ao.png       ← traditional costume
      06-khung-cui.png    ← weaving loom
      07-mu.png           ← traditional hat + jewelry
      08-phan-go.png      ← wooden bed platform
      09-mam-go.png       ← wooden tray
    /sounds (optional)
      fire.mp3
      drum.mp3
      ambient.mp3
```

---

## 4. Layout & Scene Structure

```
┌─────────────────────────────────────────────────┐
│  HEADER: Title + counter (X/9) + sound toggle   │
├─────────────────────────────────────────────────┤
│                                                 │
│   SCENE CONTAINER (position: relative)          │
│   ┌─────────────────────────────────────────┐   │
│   │  room-bg.png (100% width, auto height)  │   │
│   │                                         │   │
│   │   [hotspot] [hotspot]    [hotspot]      │   │
│   │        [hotspot]  [hotspot]             │   │
│   │   [hotspot]    [hotspot]  [hotspot]     │   │
│   │              [hotspot]                  │   │
│   └─────────────────────────────────────────┘   │
│                                                 │
│   INFO PANEL (position: fixed, bottom slide-up) │
│   ┌─────────────────────────────────────────┐   │
│   │ [photo] | NAME                          │   │
│   │         | Description 1–2 sentences     │   │
│   │         | [✕ close]                     │   │
│   └─────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘
```

---

## 5. Color Palette & Typography

```css
:root {
  --bg:           #1a0f08;      /* dark warm black */
  --surface:      #f5ead8;      /* warm ivory panel */
  --earth:        #6B3A1F;      /* dark earth brown (text) */
  --gold:         #C8963E;      /* warm gold (accents, border) */
  --indigo:       #2D3A6B;      /* Lô Lô indigo (secondary) */
  --glow:         rgba(200,150,62,0.35); /* hotspot glow */
  --panel-border: #C8963E;
}
```

**Fonts (Google Fonts CDN):**
- Display/Title: `Playfair Display` (serif, warm)
- Body/Description: `Lora` (readable serif, editorial)

```html
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700&family=Lora:wght@400;500&display=swap" rel="stylesheet">
```

---

## 6. Artifact Data (hardcoded JS object)

```javascript
const ARTIFACTS = [
  {
    id: "bep-lua",
    name: "Bếp Lửa",
    nameEn: "Sacred Fireplace",
    description: "Bếp lửa là trung tâm của không gian sinh hoạt trong ngôi nhà truyền thống, nơi gắn với việc nấu ăn, sưởi ấm và quây quần gia đình. Ngọn lửa chưa bao giờ tắt.",
    photo: "assets/artifacts/01-bep-lua.png",
    // Position as % of scene container (top-left origin)
    x: 18,   // left: 18%
    y: 72,   // top: 72%
    sound: "fire",
    specialEffect: "flame-burst" // CSS class added on click
  },
  {
    id: "noi-dong",
    name: "Nồi Đồng",
    nameEn: "Bronze Pot",
    description: "Nồi đồng là vật dụng quen thuộc trong gian bếp truyền thống, gợi cảm giác đời sống gia đình mộc mạc và giản dị. Màu đồng nâu sẫm pha ánh vàng cũ.",
    photo: "assets/artifacts/02-noi-dong.png",
    x: 22,
    y: 65,
    sound: null,
    specialEffect: "smoke-puff"
  },
  {
    id: "gui-tre",
    name: "Gùi Tre",
    nameEn: "Bamboo Basket",
    description: "Gùi tre là vật dụng quen thuộc trong đời sống của người dân vùng cao, thường dùng để mang đồ, đựng nông sản hoặc vật dụng hằng ngày.",
    photo: "assets/artifacts/03-gui-tre.png",
    x: 30,
    y: 68,
    sound: null,
    specialEffect: "sway"
  },
  {
    id: "trong-dong",
    name: "Trống Đồng",
    nameEn: "Bronze Drum",
    description: "Trống đồng là biểu tượng văn hóa đặc trưng, gắn với đời sống tinh thần và bản sắc của cộng đồng Lô Lô. Tiếng trống vang lên mỗi dịp lễ hội.",
    photo: "assets/artifacts/04-trong-dong.png",
    x: 14,
    y: 60,
    sound: "drum",
    specialEffect: "ripple"
  },
  {
    id: "vay-ao",
    name: "Váy Áo Lô Lô",
    nameEn: "Traditional Costume",
    description: "Váy áo Lô Lô là trang phục truyền thống đặc trưng nhất, được may thủ công từ vải thổ cẩm với hoa văn thêu tay tinh xảo, màu sắc rực rỡ mang đậm bản sắc.",
    photo: "assets/artifacts/05-vay-ao.png",
    x: 75,
    y: 38,
    sound: null,
    specialEffect: "pattern-glow"
  },
  {
    id: "khung-cui",
    name: "Khung Cửi",
    nameEn: "Weaving Loom",
    description: "Khung cửi dệt vải là dụng cụ truyền thống quan trọng nhất trong văn hóa người Lô Lô Chải. Mỗi tấm vải dệt ra đều gắn liền với trang phục và lễ hội.",
    photo: "assets/artifacts/06-khung-cui.png",
    x: 68,
    y: 62,
    sound: null,
    specialEffect: "thread-weave"
  },
  {
    id: "mu-trang-suc",
    name: "Mũ & Trang Sức",
    nameEn: "Hat & Silver Jewelry",
    description: "Mũ truyền thống và trang sức bạc là 'báu vật' không thể thiếu của phụ nữ Lô Lô — vòng cổ to bản, khuyên tai dài, lắc tay chế tác thủ công tinh xảo.",
    photo: "assets/artifacts/07-mu.png",
    x: 80,
    y: 28,
    sound: null,
    specialEffect: "sparkle"
  },
  {
    id: "phan-go",
    name: "Phản Gỗ",
    nameEn: "Wooden Platform Bed",
    description: "Phản gỗ thấp là nơi ngồi nghỉ và sum vầy giản dị nhưng tràn đầy hơi ấm của gia đình người Lô Lô Chải sau ngày làm nương rẫy.",
    photo: "assets/artifacts/08-phan-go.png",
    x: 50,
    y: 60,
    sound: null,
    specialEffect: "sway"
  },
  {
    id: "mam-go",
    name: "Mâm Gỗ",
    nameEn: "Wooden Tray",
    description: "Mâm gỗ là vật dụng quen thuộc trong bữa ăn gia đình người Lô Lô Chải, tượng trưng cho sự đoàn kết, chia sẻ và hiếu khách của cộng đồng.",
    photo: "assets/artifacts/09-mam-go.png",
    x: 48,
    y: 72,
    sound: null,
    specialEffect: null
  }
];
```

> **Note for agent:** `x` and `y` values above are placeholder percentages. These MUST be adjusted after seeing the actual room background image. Use `position: absolute; left: {x}%; top: {y}%` on each hotspot element.

---

## 7. Hotspot Component

**HTML structure per hotspot (generated by JS):**
```html
<div class="hotspot" data-id="bep-lua" style="left: 18%; top: 72%;">
  <div class="hotspot-ring"></div>
  <div class="hotspot-dot"></div>
</div>
```

**CSS:**
```css
.hotspot {
  position: absolute;
  width: 48px;
  height: 48px;
  transform: translate(-50%, -50%);
  cursor: pointer;
  z-index: 10;
}

.hotspot-dot {
  width: 16px;
  height: 16px;
  background: var(--gold);
  border-radius: 50%;
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 0 12px var(--glow);
}

.hotspot-ring {
  width: 48px;
  height: 48px;
  border: 2px solid var(--gold);
  border-radius: 50%;
  position: absolute;
  opacity: 0.6;
  animation: pulse-ring 2s ease-out infinite;
}

@keyframes pulse-ring {
  0%   { transform: scale(0.8); opacity: 0.6; }
  100% { transform: scale(1.4); opacity: 0; }
}

.hotspot.discovered .hotspot-dot {
  background: #888; /* grey out after found */
  box-shadow: none;
}
.hotspot.discovered .hotspot-ring {
  animation: none;
  opacity: 0;
}
```

---

## 8. Animation Spec (GSAP)

### 8.1 Hover effect
```javascript
hotspotEl.addEventListener("mouseenter", () => {
  gsap.to(hotspotEl.querySelector(".hotspot-dot"), {
    scale: 1.4,
    duration: 0.3,
    ease: "back.out(2)"
  });
});
hotspotEl.addEventListener("mouseleave", () => {
  gsap.to(hotspotEl.querySelector(".hotspot-dot"), {
    scale: 1,
    duration: 0.3,
    ease: "power2.out"
  });
});
```

### 8.2 Click — Phase 1: artifact response (0–0.3s)
```javascript
// Clicked artifact bounces
gsap.timeline()
  .to(hotspotEl, { scale: 1.3, duration: 0.1, ease: "power2.out" })
  .to(hotspotEl, { scale: 0.9, duration: 0.1 })
  .to(hotspotEl, { scale: 1.0, duration: 0.1 });

// Dim all OTHER hotspots
gsap.to(".hotspot:not(.active)", { opacity: 0.3, duration: 0.3 });
```

### 8.3 Click — Phase 2: panel slide-up (0.3–0.6s)
```javascript
gsap.fromTo("#info-panel",
  { y: "100%", opacity: 0 },
  { y: "0%", opacity: 1, duration: 0.35, ease: "power3.out", delay: 0.2 }
);
```

### 8.4 Click — Phase 3: content fade-in (0.6–1.0s)
```javascript
const tl = gsap.timeline({ delay: 0.5 });
tl.fromTo("#panel-photo",  { opacity: 0 }, { opacity: 1, duration: 0.2 })
  .fromTo("#panel-name",   { x: -20, opacity: 0 }, { x: 0, opacity: 1, duration: 0.2 })
  .fromTo("#panel-desc",   { y: 10, opacity: 0 },  { y: 0, opacity: 1, duration: 0.25 });
```

### 8.5 Close panel
```javascript
gsap.to("#info-panel", {
  y: "100%", opacity: 0, duration: 0.3, ease: "power2.in",
  onComplete: () => {
    gsap.to(".hotspot", { opacity: 1, duration: 0.3 });
  }
});
```

---

## 9. Info Panel HTML

```html
<div id="info-panel">
  <button id="panel-close">✕</button>
  <div id="panel-content">
    <div id="panel-photo-wrap">
      <img id="panel-photo" src="" alt="" />
    </div>
    <div id="panel-text">
      <h2 id="panel-name"></h2>
      <p id="panel-name-en"></p>
      <p id="panel-desc"></p>
    </div>
  </div>
</div>
```

**CSS:**
```css
#info-panel {
  position: fixed;
  bottom: 0; left: 50%;
  transform: translateX(-50%) translateY(100%);
  width: min(680px, 95vw);
  background: var(--surface);
  border: 1.5px solid var(--gold);
  border-bottom: none;
  border-radius: 20px 20px 0 0;
  padding: 28px 32px;
  box-shadow: 0 -8px 40px rgba(0,0,0,0.4);
  z-index: 100;
}

#panel-content {
  display: flex;
  gap: 24px;
  align-items: flex-start;
}

#panel-photo-wrap {
  flex-shrink: 0;
  width: 120px; height: 120px;
  border-radius: 12px;
  border: 2px solid var(--gold);
  overflow: hidden;
}

#panel-photo { width: 100%; height: 100%; object-fit: cover; }

#panel-name {
  font-family: 'Playfair Display', serif;
  font-size: 1.3rem;
  color: var(--earth);
  margin: 0 0 4px;
}

#panel-name-en {
  font-family: 'Lora', serif;
  font-size: 0.8rem;
  color: var(--gold);
  margin: 0 0 12px;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}

#panel-desc {
  font-family: 'Lora', serif;
  font-size: 0.95rem;
  color: #4a3020;
  line-height: 1.65;
  margin: 0;
}

#panel-close {
  position: absolute;
  top: 16px; right: 20px;
  background: none;
  border: none;
  font-size: 1.2rem;
  color: var(--earth);
  cursor: pointer;
  opacity: 0.6;
}
#panel-close:hover { opacity: 1; }
```

---

## 10. Counter & Header

```html
<header id="site-header">
  <h1>Ngôi Nhà Lô Lô Chải</h1>
  <div id="counter">
    <span id="found-count">0</span>
    <span> / 9 vật phẩm</span>
  </div>
  <button id="sound-toggle">🔊 Sound</button>
</header>
```

**Counter update logic:**
```javascript
let discoveredCount = 0;
const discovered = new Set();

function onArtifactClick(id) {
  if (!discovered.has(id)) {
    discovered.add(id);
    discoveredCount++;
    document.getElementById("found-count").textContent = discoveredCount;
    document.querySelector(`[data-id="${id}"]`).classList.add("discovered");
  }
  openPanel(id);
}
```

---

## 11. Core JS Logic (complete flow)

```javascript
// 1. Render all hotspots onto scene
function initHotspots() {
  const scene = document.getElementById("scene");
  ARTIFACTS.forEach(artifact => {
    const el = document.createElement("div");
    el.className = "hotspot";
    el.dataset.id = artifact.id;
    el.style.left = artifact.x + "%";
    el.style.top  = artifact.y + "%";
    el.innerHTML = `<div class="hotspot-ring"></div><div class="hotspot-dot"></div>`;
    el.addEventListener("mouseenter", () => onHover(el, true));
    el.addEventListener("mouseleave", () => onHover(el, false));
    el.addEventListener("click", () => onArtifactClick(artifact.id));
    scene.appendChild(el);
  });
}

// 2. Open panel with artifact data
function openPanel(id) {
  const artifact = ARTIFACTS.find(a => a.id === id);
  document.getElementById("panel-photo").src    = artifact.photo;
  document.getElementById("panel-photo").alt    = artifact.name;
  document.getElementById("panel-name").textContent   = artifact.name;
  document.getElementById("panel-name-en").textContent = artifact.nameEn;
  document.getElementById("panel-desc").textContent   = artifact.description;
  animatePanelOpen();
}

// 3. Close panel
document.getElementById("panel-close").addEventListener("click", animatePanelClose);
document.getElementById("info-panel").addEventListener("click", (e) => {
  if (e.target === document.getElementById("info-panel")) animatePanelClose();
});

// 4. Init on load
window.addEventListener("DOMContentLoaded", initHotspots);
```

---

## 12. Special Effects Per Artifact (CSS classes)

| Artifact | Effect class | Implementation |
|---|---|---|
| Bếp lửa | `flame-burst` | Scale hotspot-dot 1.5× + orange glow pulse for 0.5s |
| Trống đồng | `ripple` | Radial box-shadow expanding ring from hotspot center |
| Váy áo | `pattern-glow` | Rainbow gradient border animation on hotspot |
| Mũ & trang sức | `sparkle` | 3 small gold dots animate outward from hotspot |
| Gùi, Phản gỗ | `sway` | translateX(-5px → 5px → 0) over 0.4s |
| Others | none | Standard bounce only |

These are enhancement — implement ONLY after core flow works.

---

## 13. Build & Delivery

**For demo (simplest):**
```
index.html       ← everything embedded
/assets/...      ← image files
```
Open with `live-server` or any static server. No build step needed.

**If agent uses Vite:**
```bash
npm create vite@latest lo-lo-chai -- --template vanilla
cd lo-lo-chai
npm install gsap
# copy assets, replace main.js with logic above
npm run dev
```

---

## 14. What Agent Should NOT Build

- No routing, no multi-page
- No mobile responsive (demo only)
- No form or contact section
- No CMS integration
- No real sound files needed — stub Howler calls with `console.log("sound:", name)`
- No SEO meta tags
- No loading screen (unless trivially simple)

---

## 15. Acceptance Criteria (Definition of Done)

- [ ] Room background image fills scene container correctly
- [ ] All 9 hotspots render at correct positions with pulsing ring animation
- [ ] Hover → dot scales up
- [ ] Click → bounce animation → other hotspots dim → panel slides up
- [ ] Panel shows correct photo + name + description for clicked artifact
- [ ] Close button → panel slides down → all hotspots return to full opacity
- [ ] Counter increments (only once per artifact)
- [ ] Discovered hotspots turn grey (no re-trigger pulse)
- [ ] No console errors
- [ ] Works in Chrome/Edge without build step (CDN imports only)
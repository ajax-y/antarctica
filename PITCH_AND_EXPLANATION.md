# Eco-Scan: First Round Pitch & Technical Presentation Guide

> **Project Name:** Eco-Scan  
> **Tagline:** Product Carbon & Plastic Footprint Analyzer  
> **Live Web App:** [https://ajax-y.github.io/antarctica/](https://ajax-y.github.io/antarctica/)  
> **GitHub Repository:** [https://github.com/ajax-y/antarctica](https://github.com/ajax-y/antarctica)

---

## 1. Hook & Problem Statement (The "Why")

### The Problem:
Every day, consumers buy packaged retail items (chips, soaps, shampoos, noodles, water bottles) without knowing their true environmental cost.
- Nutritional information (calories, fat, sugar) is legally mandated and printed on every pack.
- **Environmental packaging impact (plastic grams, cradle-to-grave $\text{CO}_2$, decomposition half-life)** is completely invisible to the shopper.
- As a result, sustainable purchasing decisions remain nearly impossible for the average consumer.

### The Solution:
**Eco-Scan** is a camera-enabled web tool that turns any product barcode into an **immediate environmental impact report**:
1. Scans the barcode in milliseconds using the device's camera.
2. Identifies packaging materials (PET, MLP foil laminates, cardboard, aluminium).
3. Computes lifecycle carbon emissions ($\text{g CO}_2e$) and marine pollution severity.
4. Suggests an actionable, cleaner eco-alternative with potential monthly savings ($₹$).
5. Projects what happens if an entire city (Chennai) or state (Tamil Nadu) switches.

---

## ⭐ The Novelty Factor (Why Eco-Scan is Not Just a Passive Scorecard)

> **Jury Defense:** *"Other tools only display a passive rating or score. Eco-Scan provides direct prevention, legal accountability, and circular disposal closing the loop from awareness to action."*

1. **Active Waste Prevention Quota Tracker (The Eco-Swap Action):**
   - Users don't just look at plastic numbers; they click **"✓ LOG PLASTIC PREVENTED"** to record an active swap.
   - The app logs the negative grams diverted into a permanent session ledger, proving active behavioral change.

2. **Legal EPR (Extended Producer Responsibility) Accountability:**
   - Under India's **Plastic Waste Management Rules (2022/EPR)**, FMCG brands are legally obligated to take back 100% of their flexible plastic packaging.
   - Eco-Scan automatically generates a direct **brand accountability demand** with one click (Twitter/X intent), asking the specific manufacturer where citizens in Chennai can drop off their non-recyclable wrappers.

3. **Material-Specific Safe Disposal Protocol:**
   - For items already purchased, Eco-Scan categorizes the polymer kinetics:
     - **MLP (Multi-Layer Plastic):** Segregation guide for **Eco-Bricks** (building blocks for green construction) and cement kiln co-processing.
     - **PET & Cans:** Direct GCC (Greater Chennai Corporation) Dry Waste Collection Centers (DWCC) drop-off directory with zone-by-zone units.

---

## 2. What the Product Does (Core Capabilities)

When a product is scanned, Eco-Scan instantly renders five key insights:

1. **Instant Material Diagnostics:**
   - **Plastic Mass:** Weight of synthetic plastic resin in that single unit (e.g. 5.4g in a soap pack or 18.5g in a 1L PET bottle).
   - **Lifecycle Carbon Footprint ($\text{g CO}_2e$):** Cradle-to-grave emissions (crude oil extraction $\rightarrow$ polymer refining $\rightarrow$ packaging manufacturing $\rightarrow$ freight).
   - **Pollution Severity:** Rates ecological risk (e.g. multi-layer plastic cannot be separated or recycled in municipal facilities).
   - **Decomposition Time:** Estimates degradation years (e.g. 450–500 years for MLP/PET).

2. **Eco-Score Index ($0–100$):**
   - High-contrast visual metric:
     - `0–39`: High Pollution (Single-use plastics, MLPs)
     - `40–74`: Moderate Impact (Aluminium cans, Tetra cartons)
     - `75–100`: Sustainable (Compostable paper, bulk refills)

3. **Macro-Scale Impact Projections (The Multiplier Effect):**
   - Single-unit numbers like "15g of plastic" feel small to an individual.
   - Eco-Scan allows toggling between:
     - **Individual:** Monthly footprint (12 units/mo).
     - **Chennai Metropolis:** 1,000,000 active urban consumers ($12\times 10^6\text{ units/mo}$).
     - **Tamil Nadu State:** 10,000,000 regional consumers ($120\times 10^6\text{ units/mo}$).
   - Translates abstract grams into:
     - **Metric Tonnes** ($\text{MT}$) of diverted plastic.
     - **Tonnes of $\text{CO}_2e$** abated.
     - **Mature Tree Equivalents** (annual carbon absorption of trees).
     - **10-Tonne Garbage Trucks Diverted** from Kodungaiyur & Perungudi dump yards.

4. **Actionable Eco-Friendly Alternative & Clear Comparison Chart:**
   - Recommends circular alternatives (e.g., refillable stainless steel flask, loose bulk groceries, unpackaged soap bars).
   - Provides a side-by-side **horizontal comparison chart**:
     - 🔴 **Red Bar:** Current Product (Pollution created).
     - 🟢 **Green Bar:** Eco Alternative (Cleaner choice).
     - Simple takeaway: *Lower is better.*

5. **Scan History Ledger:**
   - Automatically maintains a session log in browser storage (`localStorage`) displaying cumulative plastic and carbon saved across all scanned items.

---

## 3. Where the Data Comes From (Data Architecture)

> **Key Pitch Point for Jury:** Eco-Scan is **100% dynamic** with **zero hardcoded brand dictionaries or mock tables**.

The data pipeline runs on a two-tier hybrid architecture:

### Tier 1: Real-Time Distributed API Mesh
When a barcode is entered, Eco-Scan concurrently queries public open registries:
- **Open Food Facts (India & Global API):** Over 3 million crowdsourced food and beverage products providing packaging material hierarchies, brand owners, and eco-scores.
- **Open Beauty Facts API:** Personal care, soaps, shampoos, and cosmetic packaging data.
- **Open Products Facts API:** Packaged household FMCG items.
- **UPCitemdb Open Catalog API:** General consumer packaged goods (hardware, stationery, kitchenware).

We use `Promise.any()` with `AbortController` timeouts (2.5s) so whichever API responds first immediately resolves the product name, brand, and verified photos.

### Tier 2: Deterministic Mathematical LCA Engine (Sub-15ms Fallback)
If a brand-new or unlisted barcode is scanned, or if the user is in an offline spot:
- The system generates an immediate deterministic mathematical Life Cycle Assessment (LCA) using standard polymer physics:
  - **Polyethylene Terephthalate (PET):** Base mass $\approx 18.5\text{g}$, carbon factor $8.8\text{ g CO}_2/\text{g}$.
  - **Multi-Layer Polymer Laminate (MLP / BoPP):** Base mass $\approx 4.5\text{g}$, carbon factor $24.5\text{ g CO}_2/\text{g}$.
  - **Paperboard / Carton:** Base mass $\approx 0.8\text{g}$, carbon factor $3.5\text{ g CO}_2/\text{g}$.
  - **Glass / Aluminium:** Infinitely recyclable, circular scrap recovery model.
- **Result:** The user **never** experiences a stuck UI, broken page, or blank screen.

---

## 4. Technical Stack & Implementation

| Layer | Technology | Why We Chose It |
|---|---|---|
| **Frontend Framework** | **Vanilla JavaScript (ES2022+)** | Zero build-step dependencies, zero framework overhead, sub-millisecond DOM manipulation, instant loading. |
| **Styling** | **Tailwind CSS + Swiss Grid System** | High contrast, minimalist, clean card-based layout with strict mobile-first responsive stacking. |
| **Typography** | **`Inter` & `JetBrains Mono`** | Clean separation: `Inter` for human-readable copy; `JetBrains Mono` strictly for numerical data and barcode digits. |
| **Optical Scanner** | **`html5-qrcode` (WebRTC)** | Real-time in-browser camera barcode acquisition supporting `EAN-13`, `EAN-8`, `UPC-A`, `UPC-E`, `CODE-128`. |
| **Data Visualization** | **Chart.js** | Interactive HTML5 canvas comparison chart with plain-English tooltips. |
| **Persistence** | **HTML5 Web Storage (`localStorage`)** | Local device ledger preserving scan history across page reloads without requiring user login or backend databases. |
| **Deployment** | **GitHub Pages (HTTPS)** | Free, fast static web hosting with global CDN and mandatory HTTPS (required for camera access). |

---

## 5. Live Demo Script (Step-by-Step for the Jury)

**Time required:** ~2 minutes

### Step 1: Open the App
1. Open the live link: [https://ajax-y.github.io/antarctica/](https://ajax-y.github.io/antarctica/)
2. Point out the clean UI:
   - Header with status indicator (`SYSTEM: AWAITING SCAN`).
   - Clear hero heading and dual barcode acquisition (Live Camera Scan + Direct Number Entry).

### Step 2: Scan or Enter a Sample Barcode
Enter **`8901058851428`** (Maggi Noodles) or **`8901023011603`** (Cinthol Soap) and hit **Enter** or **ANALYZE →**.

### Step 3: Walk Through the Results
- **Sustainability Analysis:** Show the single-pack plastic mass, carbon emissions, and decomposition half-life (400+ years).
- **Macro-Scale Impact:** Click **Chennai (1M)** or **Tamil Nadu (10M)**.
  - *Say to the jury:* "A single packet is only a few grams, but across Chennai, switching this product prevents over **60 metric tonnes** of plastic waste and diverts dozens of garbage trucks from city dump yards."
- **Eco Alternatives & Chart:** Show the clear comparison chart:
  - Red bar = Current Product
  - Green bar = Sustainable Alternative
- **Scan History:** Scroll down to the clean ledger showing the session total.

---

## 6. Likely Jury Questions & How to Answer

### Q1: "Is this data hardcoded in your app?"
> **Answer:** "No, absolutely not. The app is 100% dynamic. We connect directly to the global Open Food Facts, Open Beauty Facts, and UPCitemdb open API mesh over live HTTPS. If a product is scanned, its crowdsourced information is pulled live. For unlisted items, we use a deterministic material science LCA formula based on international polymer standards (PET, MLP, HDPE)."

### Q2: "Why did you build this as a web app instead of a native mobile app?"
> **Answer:** "Friction is the biggest barrier to sustainability. If a shopper has to download a 50MB app from the Play Store while standing in a grocery aisle, they won't do it. With a lightweight web app, any user can open a browser link or scan a QR code at a supermarket shelf, get instant camera access via WebRTC, and view their impact in under 2 seconds without installing anything."

### Q3: "What makes your project unique compared to generic barcode scanners?"
> **Answer:** "Standard barcode scanners only give you product prices or ingredient calories. Eco-Scan focuses strictly on **material lifecycle physics** and **collective impact**. By translating small numbers into metric tonnes and municipal garbage trucks, it turns personal shopping into a visible community climate action."

---

## 7. Future Roadmap (If asked about Next Steps)
1. **PWA (Progressive Web App):** Add service workers for full offline caching.
2. **Supermarket Integration:** API hook for grocery e-commerce (Blinkit, Zepto, BigBasket) to suggest eco-swaps right in the online cart.
3. **Consumer Carbon Footprint Passports:** QR-based digital product passports compliant with upcoming extended producer responsibility (EPR) regulations.

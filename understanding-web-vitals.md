# Understanding Web Vitals

## Part 1 – Concepts

### What are Web Vitals vs Core Web Vitals

**Web Vitals**  
Web Vitals are a set of performance metrics introduced by Google to measure how fast, responsive, and visually stable a website feels to real users. They focus on:
- **Loading experience** → How quickly something meaningful appears.
- **Interactivity** → How soon the page responds to user actions.
- **Visual stability** → Whether things jump around unexpectedly.

**Core Web Vitals**  
Core Web Vitals are the most important Web Vitals that Google considers essential for good user experience and uses in search ranking signals:
- **LCP** – Largest Contentful Paint (Loading)
- **INP** – Interaction to Next Paint (Interactivity, replaced FID(First Input Delay) in 2024)
- **CLS** – Cumulative Layout Shift (Visual Stability)

---

### **1. FCP (First Contentful Paint)**
- **What it measures**: Time until *any* content (text, image, SVG) appears on screen.  
- **Focus**: First visual feedback — “something is happening.”  
- **User perception**: “The site has started loading.”

---

### **2. LCP (Largest Contentful Paint)**
- **What it measures**: Time until the *largest visible element* (hero image, video, big heading) is fully rendered.  
- **Focus**: Main content visibility — “the core content is ready.”  
- **User perception**: “The page is basically loaded and usable.”

---

### **3. TBT (Total Blocking Time)**
- **What it measures**: How long the main thread is blocked (in ms) between FCP and full interactivity.  
- **Focus**: Responsiveness — can the page react to clicks, scrolls, typing?  
- **User perception**: “The page looks loaded but feels frozen.”

---

### **4. CLS (Cumulative Layout Shift)**
- **What it measures**: Sum of unexpected layout movements during load.  
- **Focus**: Visual stability — preventing content from “jumping.”  
- **User perception**: “I can click without the button moving away.”

---

### **5. SI (Speed Index)**
- **What it measures**: How quickly visible content is *painted* during load, averaged over time.  
- **Focus**: Overall perceived load speed — not just first or largest paint.  
- **User perception**: “The page fills in smoothly.”

---

### **6. Performance Score**
- **What it measures**: Weighted Lighthouse score (0–100) based on metrics like FCP, LCP, TBT, CLS, SI.  
- **Focus**: Single-number summary of performance.  
- **User perception**: A quick “grade” for speed.

---

### **7. Accessibility**
- **What it measures**: Compliance with accessibility best practices (color contrast, ARIA labels, keyboard navigation).  
- **Focus**: Usability for people with disabilities.  
- **User perception**: “Can I use this site if I have vision or mobility issues?”

---

### **8. Best Practices**
- **What it measures**: General coding, security, and modern API usage (HTTPS, safe JS, responsive design).  
- **Focus**: Quality and security of implementation.  
- **User perception**: Not directly felt, but impacts trust, safety, and compatibility.

---

### **9. SEO (Search Engine Optimization)**
- **What it measures**: Checks whether your page meets **technical and content best practices** for discoverability and indexability in search engines. Focuses on metadata, mobile readiness, crawlability, and page structure.  
- **What Lighthouse checks for (examples)**:  
  1. **Meta tags**: `<title>` and `<meta name="description">` present and relevant.  
  2. **Mobile viewport tag**: `<meta name="viewport" content="width=device-width, initial-scale=1">`.  
  3. **Descriptive link text** instead of “click here.”  
  4. **Crawlability**: Not blocked by robots.txt or `noindex`.  
  5. **HTTPS**: Secure protocol in use.  
  6. **No intrusive mobile popups** blocking content.  
  7. **Valid HTML** for clean parsing by search bots.  
- **Good threshold**:  
  - 90–100 → Technically well-optimized.  
  - 50–89 → Needs improvement.  
  - <50 → Major SEO issues.  
- **Common causes of poor SEO**: Missing meta tags, no viewport tag, vague anchor text, blocked pages, no HTTPS, JS-only content without SSR.  
- **Example**:  
  - *Amazon.in SEO (96)* → Proper meta tags, HTTPS, mobile-friendly, descriptive links.  
  - *Flipkart.com SEO (98)* → Similar strengths, slightly fewer vague anchor links.

---

## Part 2 – Practical Investigation

### 1. Metrics Table

| Site                  | Device  | FCP  | LCP  | TBT    | CLS   | SI    | PerformanceScore | Accessibility | Best Practices | SEO |
| --------------------- | ------- | ---- | ---- | ------ | ----- | ----- | ---------------- | ------------- | -------------- | --- |
| https://www.amazon.in | Mobile  | 2.0s | 3.4s | 890ms  | 0.12  | 5.8s  | 63               | 84            | 79             | 91  |
| https://www.amazon.in | Desktop | 0.9s | 2.9s | 420ms  | 0.05  | 3.2s  | 79               | 88            | 78             | 96  |
| https://www.flipkart.com | Mobile  | 1.9s | 3.0s | 760ms  | 0.14  | 5.1s  | 68               | 89            | 85             | 94  |
| https://www.flipkart.com | Desktop | 0.8s | 2.5s | 310ms  | 0.04  | 2.7s  | 83               | 92            | 84             | 98  |

---

### 2. Top Issues

#### Amazon – Mobile
- LCP (3.4 s) from large hero banner images.
- TBT (890 ms) due to heavy JavaScript from product carousels and tracking scripts.
- CLS (0.12) from late-loading ads and promotional banners.

#### Amazon – Desktop
- LCP (2.9 s) from large hero graphics.
- TBT (420 ms) from dynamic content scripts.
- Best Practices score low (78) → likely mixed content or outdated APIs.

#### Flipkart – Mobile
- LCP (3.0 s) from hero image and promotional sliders.
- TBT (760 ms) from third-party scripts and JS-heavy UI.
- CLS (0.14) from late layout adjustments for ads and banners.

#### Flipkart – Desktop
- LCP (2.5 s) from large product grid images.
- TBT (310 ms) from JS-based widgets.
- Minor CLS (0.04) but still from ad placeholders.

---

### 3. Root-Cause Notes
- **FCP** – Slightly delayed due to render-blocking CSS/JS and large above-the-fold media.
- **LCP** – Heavy hero and product images.
- **TBT** – Long JS execution for UI elements, carousels, and ads.
- **CLS** – Promotional banners and ads loading after main content.
- **SI** – Multiple network requests and large bundles delaying full visual load.

---

### 4. Recommendations
1. **LCP** – Compress hero/product images, serve WebP/AVIF, and preload above-the-fold assets.  
2. **TBT** – Split JS bundles, defer non-critical scripts, async load third-party tags.  
3. **CLS** – Reserve space for banners/ads, set explicit width/height for media, use `font-display: swap`.  
4. **SI** – Reduce unused CSS/JS, implement server-side rendering for key components.  
5. **General** – Enable caching and use CDN for faster delivery, upgrade to HTTP/3.

---

**Most Impactful First Recommendation:**  
For both Amazon and Flipkart: **Optimize and preload above-the-fold hero and product images** to improve LCP and overall performance.

# 🎨 PNG to SVG Logo Conversion & Vector Guidelines

> **Purpose**: A comprehensive guide for contributors, designers, and maintainers on converting raster images (PNG) to scalable vector graphics (SVG), creating resolution-independent logos, and adhering to vector-first design best practices.

---

## 📌 Table of Contents
1. [Introduction & Why Vectors Matter](#1-introduction--why-vectors-matter)
2. [The Pitfalls of Common Online PNG-to-SVG Converters](#2-the-pitfalls-of-common-online-png-to-svg-converters)
3. [Automated Vectorization: Tools & Software Parameters](#3-automated-vectorization-tools--software-parameters)
4. [Manual & Hybrid Professional Workflows](#4-manual--hybrid-professional-workflows)
5. [Designing Logos Directly in SVG (Proactive Vector-First Rules)](#5-designing-logos-directly-in-svg-proactive-vector-first-rules)
6. [Preparing PNGs for Easier Conversion Later](#6-preparing-pngs-for-easier-conversion-later)
7. [Quick Decision Tree & Summary Cheat Sheet](#8-quick-decision-tree--summary-cheat-sheet)

---

## 1. Introduction & Why Vectors Matter

When contributing logos, banners, or brand icons to projects, choice of image format directly impacts web performance, scaling quality, and maintainability.

### Raster (PNG) vs Vector (SVG)

| Attribute | Raster (PNG) | Vector (SVG) |
| :--- | :--- | :--- |
| **Structure** | Grid of fixed pixels | Mathematical paths (`<path>`, `<rect>`, `<circle>`) |
| **Scaling** | Pixelates/blurs when zoomed in | Infinitely scalable without loss of quality |
| **File Size** | Typically 150 KB – 500 KB+ for high resolution | Typically 1 KB – 10 KB for clean logo vectors |
| **Editability** | Requires image manipulation tools (Photoshop) | Easily edited in vector software or plain text/XML |
| **Web Styling** | Static pixels | CSS & JS themeable (hover effects, dark/light modes) |

> All official AOSSIE brand assets and project logos should ideally be maintained as **clean SVG vector graphics**, with PNG exports generated only as secondary fallbacks.

---

## 2. The Pitfalls of Common Online PNG-to-SVG Converters

Many free online "PNG to SVG" converters produce poor-quality files. Understanding how these tools operate helps explain why naive conversion fails.

```
                  ┌─────────────────────────────────────────┐
                  │          Input PNG Logo File            │
                  └────────────────────┬────────────────────┘
                                       │
                    ┌──────────────────┴──────────────────┐
                    ▼                                     ▼
        ┌──────────────────────┐              ┌──────────────────────┐
        │  Pseudo-Converters   │              │ Micro-Polygon Trace  │
        │  (Raster Wrappers)   │              │  (Naive Auto-Trace)  │
        └───────────┬──────────┘              └───────────┬──────────┘
                    │                                     │
                    ▼                                     ▼
        ❌ Massive size (200KB+)              ❌ Thousands of jagged paths
        ❌ Still pixelates on zoom            ❌ File size explodes (500KB+)
        ❌ No vector nodes created            ❌ Glitches on web renderers
```

### Pitfall 1: Pseudo-Conversion (Raster Container Wrapper)
- **How it works**: The tool takes your PNG, converts it into a Base64 string, and embeds it inside an SVG wrapper:
  ```xml
  <!-- ❌ BAD SVG: A raster image hidden inside SVG tags -->
  <svg xmlns="http://www.w3.org/2000/svg" width="500" height="500">
    <image href="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA..." width="500" height="500"/>
  </svg>
  ```
- **Why it fails**: 
  - File size remains identical (or ~33% larger due to Base64 encoding).
  - The image still pixelates when scaled.
  - No vector paths exist; it cannot be edited, recolored, or styled.

### Pitfall 2: Micro-Polygon Auto-Tracing (Naive Vectorizer)
- **How it works**: The tool attempts to auto-trace the image by creating thousands of tiny, overlapping polygon shapes to mimic pixel shading, gradients, or anti-aliased edges.
- **Why it fails**:
  - Explodes file size (often 500 KB to 2 MB for a simple logo).
  - Creates visual rendering gaps and edge artifacts when rendered in browsers.
  - Causes GPU stuttering and high DOM memory consumption.
  - Extremely difficult to edit or manipulate in vector tools like Figma or Illustrator.

> [!WARNING]
> Automated converters should **only** be used for extremely simple, flat-colored graphics with sharp boundaries. Complex logos require proper software tracing or manual vector reconstruction.

---

## 3. Automated Vectorization: Tools & Software Parameters

When automated conversion is appropriate (for simple, high-contrast, flat shapes), use dedicated tools with optimized settings rather than basic wrapper sites.

### Recommended Automated Tools

1. **Adobe Illustrator PC Software (Image Trace)**
2. **[Adobe Express](https://www.adobe.com/express/feature/image/convert/png-to-svg)** (Web Tool)
3. **Inkscape (Trace Bitmap)**
---

### Software Step-by-Step Instructions

#### Option A: Adobe Illustrator (Image Trace)

```
[Import PNG] ──► [Go to Window > Image Trace] ──► [Ensure that the asset is selected] ──► [In Image Trace Select Mode = Colour (Recommended)] ──► [Click Trace] ──► [Go to File > Export > Export As ] ──► [Save the File]
```

---

#### Option B: Inkscape (Trace Bitmap)

1. Open Inkscape and import your PNG image (`File > Import`).
2. Select the image and navigate to **Path > Trace Bitmap** (`Shift + Alt + B`).
3. Select the appropriate mode:
   - **Single Scan (Brightness Cutoff)**: For monochromatic/single-color logos. Set Threshold ~ `0.45`.
   - **Multiple Scans (Color Quantization)**: For multi-color logos. Set Scans to the exact number of colors in the logo (e.g., `4` or `6`). Check **Remove Background**.
4. Click **Apply**.
5. Select the resulting vector path, ungroup it (`Ctrl + Shift + G`), and run **Path > Simplify** (`Ctrl + L`) to remove redundant nodes.

---

## 4. Manual & Hybrid Professional Workflows

For professional open-source brand assets, manual or hybrid vector reconstruction yields the highest quality, smallest file sizes, and best scalability.

### Approach A: Simple Logos - Manual Recreation in Figma

> [!TIP]
> Manual recreation in Figma reduces average logo file sizes from **200 KB (PNG)** to **1–10 KB (SVG)**, providing crisp rendering at any screen resolution.
But this process may require proficieny in Figma as a software along with the ability to use Pen tool in Figma which a contributor can learn for any available tutorials or articles online.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    Manual Reconstruction in Figma                       │
├─────────────────────────────────────────────────────────────────────────┤
│ 1. Lock PNG Reference Layer (50% Opacity)                               │
│ 2. Trace Base Geometry using Primitives (Rectangle 'R', Ellipse 'O')    │
│ 3. Apply Boolean Operations (Union, Subtract, Intersect, Exclude)       │
│ 4. Fine-Tune Custom Curves using Pen Tool ('P')                         │
│ 5. Export clean SVG frame                                               │
└─────────────────────────────────────────────────────────────────────────┘
```

#### Step-by-Step Workflow:
1. **Prepare Workspace**: Create a new frame in Figma matching the desired dimensions (Currently in [Logos Hub](https://www.figma.com/design/cQCpSQv0M6FAc62ncunx8s/Logos?node-id=0-1&t=8pZG0veAJXDpYwI0-1) Figma File the standardised dimesnions are set to `360 x 360 px`).
2. **Import Reference**: Paste the PNG logo inside the frame. Lower opacity to `50%` and lock the layer (`Ctrl + Shift + L` / `Cmd + Shift + L` / Use the Lock Icon in Layers).
3. **Use Geometric Primitives First**:
   - Do not manually draw circles or rectangles with the pen tool.
   - Use **Rectangle (`R`)**, **Ellipse (`O`)**, or **Polygon** tools for exact symmetry.
4. **Combine with Boolean Operations**:
   - Select overlapping shapes and use **Union**, **Subtract**, **Intersect**, or **Exclude** from the top toolbar to create complex cutouts.
5. **Use Pen Tool (`P`) for Custom Curves**:
   - Place anchor points strictly at curve extrema (topmost, bottommost, leftmost, rightmost points of curves).
   - Keep control handles horizontal or vertical using `Shift`-drag for clean Bezier curves.
   - Minimize total node count (fewer nodes = smoother rendering and smaller SVG size).
6. **Apply Brand Colors**: Fill shapes using exact AOSSIE brand color codes (e.g., Golden Wallet `#FFCD00`, Baggy Green `#00843D`).
7. **Export**: Select the top-level Frame, set format to **SVG**, and click **Export**.

---

### Approach B: Complex Logos - Hybrid Workflow (Figma + Adobe Illustrator)

For logos containing gradients, drop shadows, inner depth, or layered effects (e.g., multi-shaded emblems):

1. **Structural Base in Figma**: Rebuild the main outlines, silhouettes, and geometric base paths in Figma using native shapes and the Pen tool.
2. **Transfer to Illustrator**: Copy vector paths into Adobe Illustrator (`Ctrl + C` / `Ctrl + V`).
3. **Recreate Vector Depth**:
   - **Gradients**: Replace raster blurs with vector **Linear/Radial Gradients** or **Freeform Gradients** (`Window > Gradient`).
   - **Contour Outlines**: Use **Offset Path** (`Object > Path > Offset Path`) instead of manual redrawing for consistent border scaling.
   - **Layer Depth**: Use stacked fill layers in the **Appearance Panel** (`Window > Appearance`).
4. **Expand Effects**: Convert all strokes and live effects into expanded vector geometry (`Object > Expand Appearance`).
5. **Export Clean Vector**: Save as optimized SVG.

---

## 5. Designing Logos Directly in SVG (Proactive Vector-First Rules)

To eliminate the need for PNG-to-SVG conversion entirely, follow these vector-first design rules when creating new logos or brand graphics.

```
       ┌───────────────────────────────────────────────────────────────┐
       │             Vector-First Design Best Practices                │
       ├───────────────────────────────────────────────────────────────┤
       │  ✅ Design directly in vector tools (Figma/Illustrator)       │
       │  ✅ Align objects to Pixel Grid (Snap to Grid)                │
       │  ✅ Convert all typography/fonts to Outlines                  │
       │  ✅ Outline all Strokes to filled paths                       │
       │  ✅ Organize layers with descriptive IDs                      │
       └───────────────────────────────────────────────────────────────┘
```

1. **Design Native Vectors**: Always create logos in Figma, Adobe Illustrator, Inkscape, or Affinity Designer. Never draw logos in raster-based software like Photoshop, Paint, or Canva (unless using vector export).
2. **Pixel Grid Alignment**: Set up a square canvas (e.g., `360 x 360 px` or `512 x 512 px`) and enable **Snap to Pixel Grid**. This prevents fractional coordinate values (e.g., `x="12.3456"`) in exported SVG code.
3. **Outline All Typography (Convert Text to Paths)**:
   - Raw text elements (`<text>Custom Font</text>`) break if the viewer does not have the font installed.
   - In Illustrator: Select text and press `Ctrl + Shift + O` (`Cmd + Shift + O`).
   - In Figma: Select text layer and press `Ctrl + E` (`Cmd + E`) or right-click > **Flatten**.
4. **Outline Strokes**:
   - Unexpanded strokes (`stroke-width="4"`) can distort when resized inside responsive containers.
   - Convert critical strokes to filled paths (`Object > Path > Outline Stroke` in Illustrator or `Outline Stroke` in Figma).
5. **Clean Layer Hierarchy**: Frame related elements and assign clean layer names (e.g., `project_logo`, `project_logomark`, `project_full_logo`). These translate directly to SVG group IDs (`<g id="project_logo">`), making the file themeable via CSS.

---

## 6. Preparing PNGs for Easier Conversion Later

If a draft image must initially be produced or exported as PNG before vectorization, follow these guidelines to make downstream vector conversion seamless:

```
                          ┌───────────────────────────┐
                          │   Ideal Draft PNG Setup   │
                          └─────────────┬─────────────┘
                                        │
           ┌────────────────────────────┼────────────────────────────┐
           ▼                            ▼                            ▼
  High Resolution              Clean Transparency            High Contrast
  • Min 2000×2000 px           • PNG-24 with Alpha           • Solid, flat colors
  • 300 DPI canvas             • No baked grid/background    • Sharp, unblurred edges
```

1. **Export at High Resolution**: Create draft canvas at a minimum of **2000 x 2000 pixels** (300 DPI). High pixel density provides sharper edges for auto-tracing algorithms.
2. **Clean Alpha Transparency**: Export as **PNG-24** with transparent background. Never bake in white backgrounds, grey card backdrops, or fake checkerboard patterns.
3. **Use Flat Colors & High Contrast**: Ensure clear visual distinction between logo elements and background.
4. **Avoid Soft Raster Effects in Drafts**:
   - Turn off drop shadows, outer glows, gaussian blurs, and lens flares in the PNG draft.
   - Soft blurs force auto-tracers to create thousands of micro-polygons. Add vector drop shadows or gradients later during the vector phase.
5. **Save Lossless**: Save with maximum quality settings without lossy compression artifacts.

---


## 7. Quick Decision Tree & Summary Cheat Sheet

Use this decision tree to select the right workflow for your logo:

```
                       Do you have a PNG logo?
                                 │
                 ┌───────────────┴───────────────┐
                 ▼                               ▼
               [YES]                           [NO]
                 │                               │
    Is it a simple shape/icon?         Design natively in SVG!
         │             │               (Figma / Illustrator)
       [YES]          [NO]                       │
         │             │                         ▼
         │             └─────────────┐   Use Vector Rules
         ▼                           ▼   (Outline text/strokes,
   Use Figma Pen Tool        Complex logo with   set viewBox, optimize)
   or Vectorizer.ai          gradients/shadows?          │
         │                           │                   │
         │                           ▼                   │
         │                   Use Hybrid Workflow         │
         │                  (Figma + Illustrator)        │
         │                           │                   │
         └───────────────────────────┼───────────────────┘
                                     │
                                     ▼
                        [AOSSIE Checklist Review]
                                     │
                                     ▼
                        ✨ Ready for PR Submission!
```

### Quick Summary

| Task | Recommended Tool | Primary Command / Action |
| :--- | :--- | :--- |
| **Simple Logo Recreation** | Figma | Native Pen Tool (`P`) + Primitives (`R`/`O`) + Booleans |
| **Quick Flat Auto-Trace** | Vectorizer.ai / SVGcode | Deep-learning vector extraction / Potrace |
| **Software Auto-Trace** | Adobe Illustrator | `Image Trace` -> Preset: `Logo` -> `Expand` |
| **Open-Source Auto-Trace**| Inkscape | `Path > Trace Bitmap` (`Shift+Alt+B`) -> `Simplify` |
| **Complex Depth/Gradients**| Figma + Illustrator | Figma base vectors + Illustrator `Freeform Gradient` / `Offset Path` |
| **SVG Code Cleanup** | SVGOMG / SVGO | Strip metadata, set precision to 2, format viewBox |

---

*© 2026 AOSSIE - Australian Open Source Software Innovation and Education*

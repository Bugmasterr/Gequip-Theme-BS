---
description: "Optimize the theme for Core Web Vitals (CLS, LCP, FID) focusing on JS, Liquid, CSS, and HTML files."
---

# Web Vitals Optimization Workflow

This workflow will guide you through optimizing your Shopify theme for Core Web Vitals.
This workflow provides specific, actionable steps for an AI assistant to optimize your Shopify theme for Core Web Vitals.


## 1. Optimize CSS for Performance and CLS

- **Identify and Remove Unused CSS**: Use a tool to find and eliminate styles that aren't being used.
- **Inline Critical CSS**: Identify the CSS needed for above-the-fold content and inline it in your `theme.liquid` file.
- **Defer Non-Critical CSS**: Load the rest of your CSS asynchronously.
- **Minify CSS**: Ensure all CSS files are minified to reduce their size.
- **Avoid `@import`**: Use `<link>` tags instead of `@import` in your CSS files as it can negatively impact performance.

Task and Actions:

- **Task: Identify and Remove Unused CSS**
  - **Action**: Systematically scan all `.css` files in the `/assets` directory. For each CSS rule, search the entire theme (`/sections`, `/snippets`, `/templates`, `/layout`) to determine if the selector is used. If a selector is not found, log it for review. Do not automatically delete.

- **Task: Inline Critical CSS for Key Templates**
  - **Action**: Identify the critical, above-the-fold CSS for the homepage, product pages, and collection pages. Create a new snippet file named `critical-css.liquid` and place this CSS inside `<style>` tags. Include this snippet in the `<head>` of `layout/theme.liquid`.

- **Task: Defer Non-Critical CSS**
  - **Action**: In `layout/theme.liquid`, modify the `<link>` tags for non-critical CSS stylesheets to load asynchronously. Change them from `<link href="{{ 'stylesheet.css' | asset_url }}" rel="stylesheet">` to `<link rel="preload" href="{{ 'stylesheet.css' | asset_url }}" as="style" onload="this.onload=null;this.rel='stylesheet'"><noscript><link href="{{ 'stylesheet.css' | asset_url }}" rel="stylesheet"></noscript>`.

- **Task: Minify All CSS Assets**
  - **Action**: Verify that all CSS files in the `/assets` directory are minified. If not, minify them.


## 2. Optimize JavaScript

- **Minify and Defer/Async JS**: Minify all JavaScript files and use `defer` or `async` attributes for non-essential scripts.
- **Remove Unused JavaScript**: Audit your JavaScript files and remove any code that is no longer necessary.
- **Optimize Third-Party Scripts**: Evaluate the performance impact of third-party scripts and load them efficiently.

Task and Actions:
- **Task: Defer or Async Non-Essential JavaScript**
  - **Action**: Review all `<script>` tags in `layout/theme.liquid` and other layout files. For scripts that are not critical for the initial page render, add the `defer` or `async` attribute. Example: `<script src="{{ 'app.js' | asset_url }}" defer></script>`.

- **Task: Audit and Remove Unused JavaScript**
  - **Action**: Analyze all `.js` files in the `/assets` directory. Trace function calls and event listeners to determine if they are used in the theme. Report any unused code blocks for manual review.

## 3. Optimize Images

- **Specify Image Dimensions**: Always include `width` and `height` attributes on `<img>` tags to prevent layout shifts (CLS).
- **Use Modern Image Formats**: Serve images in next-gen formats like WebP.
- **Lazy-Load Offscreen Images**: Use `loading="lazy"` for images that are not in the initial viewport.

Task and Actions:
- **Task: Enforce Image Dimensions**
  - **Action**: Scan all `.liquid` files for `<img>` tags. Ensure that each tag has `width` and `height` attributes. For Shopify images, use the image object attributes. Example: `<img src="{{ image | img_url: 'master' }}" width="{{ image.width }}" height="{{ image.height }}">`.

- **Task: Implement Lazy Loading for Offscreen Images**
  - **Action**: For all `<img>` tags that are likely to be offscreen (e.g., in product grids, sections further down the page), add the `loading="lazy"` attribute.

## 4. Optimize Liquid/HTML

- **Minimize DOM Size**: Keep your HTML structure as simple as possible.
- **Preload Key Resources**: Use `<link rel="preload">` for critical assets that are discovered late in the loading process.
- **Avoid Dynamic Content Injection**: Do not inject content above existing content, as this can cause layout shifts.

Task and Actions:
- **Task: Preload Key Resources**
  - **Action**: Identify critical resources like the main web font or a primary stylesheet. Add a `<link rel="preload">` tag for them in the `<head>` of `layout/theme.liquid`. Example for a font: `<link rel="preload" href="{{ 'font.woff2' | asset_url }}" as="font" type="font/woff2" crossorigin>`.

## 5. Optimize Fonts

- **Preload Web Fonts**: Preload your most important web fonts.
- **Use `font-display: swap`**: This ensures that text remains visible during webfont loading.
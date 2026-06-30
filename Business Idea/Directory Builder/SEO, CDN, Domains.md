
# SEO, CDN Caching, and Custom Domains

### Architecture Overview for the Multi‑Tenant Directory Platform

## 1. SEO Strategy Overview

The platform is designed to maximize SEO performance for all directories and listings. SEO is handled entirely at the **Next.js frontend layer**, not the backend language. The backend (Node, C#, Rust, etc.) does not influence SEO.

### Key SEO Mechanisms

- **Server‑Side Rendering (SSR)** for dynamic pages
- **Static Site Generation (SSG)** for directory and listing pages
- **Incremental Static Regeneration (ISR)** for updating pages after edits
- **Metadata API** for titles, descriptions, and OpenGraph
- **JSON‑LD structured data** for rich search results
- **Clean, semantic URLs**
- **Automatic sitemap generation**
- **Fast global delivery via CDN caching**

### Why Next.js Is Required for SEO

Next.js provides built‑in SEO tooling that other backend technologies do not offer natively:

- SSR + SSG + ISR
- Metadata API
- File‑based routing
- Image optimization
- Edge rendering
- Automatic static optimization

These features ensure that search engines receive fully rendered, crawlable HTML.

---

## 2. CDN Caching Architecture

The platform uses a global CDN (e.g., Cloudflare) to cache directory pages for maximum performance and SEO impact.

### What Gets Cached

- Directory homepages
- Listing pages
- Category pages
- Search pages (optional)
- Static assets (images, CSS, JS)

### Rendering Modes and CDN Behavior

|Rendering Mode|Use Case|CDN Behavior|
|---|---|---|
|**SSG**|Stable pages|Fully cached at edge|
|**ISR**|Listings that update|Cached, revalidated on demand|
|**SSR**|Dashboards, admin|Not cached (private)|

### Benefits

- Global low‑latency delivery
- High Lighthouse scores
- Faster crawl and indexing
- Reduced server load
- Scales to millions of pages

---

## 3. Custom Domains for Directories

Directories can optionally use their own custom domain (e.g., `suitshopsbangkok.com`).  
This feature is **restricted to paid plans**.

### Custom Domain Flow

1. Directory owner enters a custom domain.
2. Platform generates a DNS verification token.
3. User adds a TXT record to their DNS.
4. Cloudflare verifies domain ownership.
5. Platform registers the domain via Cloudflare’s **Custom Hostnames API**.
6. Cloudflare provisions SSL automatically.
7. Cloudflare routes traffic to the Next.js app.
8. CDN caching applies automatically to all pages.

### How Routing Works

Cloudflare forwards requests to the platform with the correct `Host` header:

```
Host: suitshopsbangkok.com
```

The platform resolves:

```
SELECT directory_id FROM domains WHERE domain = host
```

Then renders the correct directory using SSG/ISR.

### Why Cloudflare Is Ideal

- Automatic SSL
- Global CDN caching
- DNS management
- Cache purge API
- Used by Shopify, Webflow, Notion, Substack, Framer

---

## 4. Subdomains for Free Plans

Free directories use platform subdomains:

- `directoryname.yourplatform.com`

Advantages:

- No DNS verification
- No SSL provisioning
- No Cloudflare API usage
- Instant setup

These subdomains are also cached at the CDN edge.

---

## 5. Data Model for Domains

A simple domain table supports custom domain routing:

**domains**

- id
- directory_id
- domain
- verification_status
- ssl_status
- created_at

---

## 6. Summary

- SEO is driven by **Next.js**, not backend language choice.
- CDN caching (Cloudflare) accelerates all directory pages globally.
- ISR ensures listings stay fresh without losing static performance.
- Custom domains are supported via Cloudflare’s Custom Hostnames API.
- **Only paid plans** can attach custom domains.
- Free plans use platform subdomains.
- Architecture matches industry leaders like Shopify and Webflow.


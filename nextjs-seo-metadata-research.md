# Research: Next.js SEO Metadata & JSON-LD Best Practices for SaaS

I'm building an open source npm package to standardize SEO metadata and JSON-LD schema implementation for Next.js App Router projects. The package will be used across multiple SaaS products (AI chatbot platform, custom AI consultancy).

Please conduct deep research on:

## 1. Next.js App Router Metadata API

- Best practices for the `metadata` and `generateMetadata` exports
- Optimal patterns for dynamic metadata (product pages, blog posts, paginated content)
- How to handle: canonical URLs, alternate languages (hreflang), robots directives, OpenGraph/Twitter cards
- Common mistakes and anti-patterns to avoid
- How top Next.js sites (Vercel, Linear, Resend) structure their metadata

## 2. JSON-LD Schema.org Implementation

- Which schema types matter most for SaaS: Organization, Product, SoftwareApplication, Article, FAQPage, BreadcrumbList, WebSite, WebPage
- Correct nesting patterns (e.g., Article with author as Person, Product with offers and reviews)
- Required vs recommended properties for each schema type (per Google's documentation)
- How to validate: Google Rich Results Test, Schema Markup Validator
- Multi-schema pages: how to combine multiple JSON-LD blocks correctly

## 3. Existing Package Landscape

- Analyze: `next-seo`, `next-sitemap`, `schema-dts`, `next-mdx-remote` (frontmatter handling)
- What do they do well? What gaps exist?
- Why do many teams still roll their own solutions?
- Type safety considerations (schema-dts approach vs custom types)

## 4. Implementation Patterns

- Centralized config vs per-page overrides
- Default metadata inheritance patterns
- How to generate absolute URLs correctly (especially in static/ISR contexts)
- Image optimization for OG images (size requirements, Next.js Image integration)
- Sitemap and robots.txt integration considerations

## 5. Edge Cases & Gotchas

- Trailing slashes and canonical consistency
- Preview/draft mode SEO handling
- Client components and metadata (limitations)
- Structured data for pagination (FAQ with many items, blog archives)
- International SEO: hreflang, locale-specific schemas

## Deliverables

1. **Best practices summary** — actionable guidelines for the package API design
2. **Schema templates** — recommended JSON-LD structures for: Homepage, Product/Pricing page, Blog post, FAQ page, About/Team page
3. **API design recommendations** — what the ideal developer experience looks like
4. **Validation checklist** — how to verify implementation is correct
5. **Package comparison table** — existing solutions with pros/cons

Focus on practical, implementation-ready insights rather than generic SEO theory. Include code examples where helpful.

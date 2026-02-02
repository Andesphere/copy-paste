Next.js SEO Metadata & JSON-LD Best Practices for SaaS

1. Next.js App Router Metadata API

Next.js’s App Router provides a robust Metadata API for SEO tags, replacing older approaches like <Head> or third-party libraries (e.g. next-seo) ￼. Key best practices include:
	•	Use metadata or generateMetadata on Server Components: Define a static export const metadata in layouts/pages for constant metadata, or a export async function generateMetadata() for dynamic data (e.g. product or blog pages) ￼ ￼. Both are server-only exports – you cannot set SEO tags in Client Components or using client-side JS ￼. This means pages with "use client" cannot directly manage metadata – instead, handle SEO in a wrapping server layout or in generateMetadata before rendering content.
	•	Dynamic metadata patterns: For pages with dynamic content (e.g. /products/[id] or blog posts), use generateMetadata({ params }) to fetch data (product name, description, etc.) and return the SEO fields ￼ ￼. Next.js will stream the metadata to the <head> after rendering the page content for human users, but it ensures bots get the full <head> upfront (disabling streaming for crawlers) ￼ ￼. To avoid double-fetching the same data for page and metadata, use caching (e.g. React.cache() or memoized fetch function) so that both generateMetadata and the page can share results ￼ ￼. This ensures efficient ISR/SSR with no redundant calls.
	•	Canonical URLs & hreflang: Always specify a canonical URL for each page to prevent duplicate content issues. In Next.js, set a base URL once (usually in the root layout) via metadataBase: new URL("https://your-domain.com"), then for individual pages set alternates: { canonical: "/path/to/page" }. Next.js will combine metadataBase and the path correctly (avoiding double slashes) ￼. For future i18n support, include alternates.languages mapping locale codes to their URLs ￼ ￼. For example, alternates: { canonical: "/pricing", languages: { "en-US": "/en-US/pricing", "de-DE": "/de-DE/pricing" } } will output the canonical and hreflang <link> tags automatically ￼. Make sure to use a consistent trailing slash style in your URLs (Next.js by default drops trailing slashes ￼) and match that in canonical links to avoid mismatches.
	•	Robots meta directives: Use the robots field in the metadata object to control indexing. Next.js supports a structured format for robots meta. For example, robots: { index: false, follow: false } will output <meta name="robots" content="noindex, nofollow"> ￼. You can also specify robots.googleBot for Googlebot-specific directives (like max-image-preview) ￼. In a draft/preview mode, it’s wise to return index: false (noindex) so that unpublished content isn’t crawled. The Metadata API makes this easy by conditionally including robots in generateMetadata when a preview flag is detected.
	•	OpenGraph & Twitter cards: Next.js Metadata API has built-in support for social cards via openGraph and twitter objects. You can specify title, description, images, etc. under these keys, and Next.js will generate the proper <meta property="og:..."> and <meta name="twitter:..."> tags ￼ ￼. Avoid duplication: If your Open Graph and Twitter content are the same as the page meta, you don’t need to repeat them. Next.js will fall back to the top-level title/description and even use Open Graph images for Twitter if not explicitly set ￼ ￼. Only override openGraph/twitter fields when you need distinct values (e.g. a different title for social sharing) ￼. This prevents the common mistake of duplicating fields in code. For instance, just setting a top-level title and description will automatically apply to OG and Twitter tags unless you provide a different value ￼.
	•	Common mistakes to avoid: Do not mix old approaches with the new API – for example, using next-seo or manually inserting <Head> tags in App Router can lead to conflicts or missing tags ￼. Always define SEO in the Metadata API for consistency ￼. Don’t forget to set metadataBase – without it, Next.js can’t resolve relative URLs for canonical or OG images, resulting in incorrect tags ￼. Also ensure every page has a unique title and meta description (no defaults left on dynamic pages). Missing canonical links and missing Open Graph images are noted as frequent issues in Next.js projects ￼. By structuring metadata in a layout hierarchy, you can provide sensible defaults (site name, default description, etc.) and override on a per-page basis as needed ￼. For example, a Root Layout might export a default title template (%s | MySite) and description, which child pages override with their specific content ￼ ￼.
	•	Examples from top Next.js sites: High-profile Next.js-powered sites (like Vercel’s own site, or apps like Linear) adhere to these patterns. They leverage the Metadata API for consistent SEO tags: e.g. setting a global base URL and defaults in the root layout, and using generateMetadata in each page to inject page-specific title, description, and canonical links. This yields clean, unified <head> tags across the app. By following the official recommendations (never using client-side head manipulation, always including canonical, etc.), these sites ensure optimal crawlability and sharing previews ￼ ￼. Emulating such structure will help your package promote best practices out of the box.

2. JSON-LD Schema.org Implementation

Structured data (JSON-LD) is vital for SaaS sites to enhance search results and provide machine-readable info about your content ￼. Key schema types for a SaaS platform include:
	•	Organization – Represents your company. Include @type: "Organization", with properties like name, url, and logo. You can also add sameAs (links to social profiles) to enrich the Knowledge Panel. For SaaS, the Organization schema is often placed on the homepage or in a global layout. Recommended properties: name (required), URL, logo, contact info, sameAs (for social URLs). This helps establish your brand identity to search engines.
	•	WebSite – Represents your overall website, mainly for enabling the sitelinks search box. Use @type: "WebSite", with name and url of your site, and a potentialAction of type SearchAction pointing to your search page URL pattern. For example: a SearchAction with "target": "https://example.com/search?query={search_term_string}" and "query-input": "required name=search_term_string". This can make Google display a search box for your site in results ￼. Typically WebSite schema is placed on the homepage. Ensure the url matches your canonical domain and include "inLanguage" if relevant.
	•	WebPage – A general schema type for a webpage, often used if no more specific type applies. It can be used with @type: "WebPage" plus a name (title of the page) and description. In practice, if you already have a more specific schema (Article, FAQPage, etc.), a separate WebPage schema isn’t strictly necessary for rich results. But you can use subtypes like "AboutPage" on an About Us page or "ContactPage", etc., to semantically classify those pages. This is mostly for completeness; Google’s rich result guidelines don’t require a WebPage schema in most cases.
	•	Product – Useful for SaaS product or pricing pages, especially if you offer a software product with subscription plans. @type: "Product" can describe your software offering. Important properties include name, description, image (one or more), and crucially an offers property. offers should be an Offer schema with details like price, priceCurrency, and availability. If your SaaS has multiple plans, you might have multiple Offer entries or use aggregateRating and review if you have user ratings. Google’s rich result guidelines for Product snippets require at minimum the product name and either reviews or aggregateRating or offers data to be eligible ￼. In practice, for a SaaS, including price (monthly fee) as an Offer and perhaps an aggregateRating (if you have user reviews/testimonials) can qualify your snippet for rating stars and price info in search ￼ ￼. If your SaaS is software-only (no physical product), you can also use SoftwareApplication schema in addition or instead – it’s similar to Product but has software-specific fields (e.g. operatingSystem, applicationCategory). However, Google doesn’t currently show special rich results for SoftwareApplication, so many SaaS companies just use Product schema for consistency with review and pricing features.
	•	Article/BlogPosting – For content marketing, blog posts or news. Use @type: "Article" (or the more specific "BlogPosting" for blog articles). Provide headline (title of the post) ￼, image (URL to a representative image), datePublished and dateModified (ISO 8601 dates) ￼, author (as a Person or Organization with name), and publisher if applicable (often the Organization with logo). Google does not enforce required properties for Article, but to maximize eligibility for rich features (e.g. appearing in Top Stories or carousels), include a concise headline, an author name, and publication date, plus an image ￼. These help Google display rich snippet info like the author and date. Nesting: the Article can have an author property that is a Person schema, and you can reuse your Organization as the publisher. Ensure the headline is ≤ 110 characters to avoid truncation in some contexts, and that the image has proper dimensions (1200px width is a good rule for social and rich result images).
	•	FAQPage – For FAQ pages with questions and answers. Use @type: "FAQPage" and provide a mainEntity array. Each item in mainEntity is a Question with a name (the question text) and an acceptedAnswer with @type: "Answer" and an text property for the answer content ￼. Google’s rich result for FAQ requires at least two Q&A pairs on the page to be eligible (and no more than around 8-10 is recommended to avoid overloading) ￼. Only mark up real FAQs (don’t use FAQPage for things like product Q&A or forums). This structured data can get your FAQs displayed as expandable questions in search results.
	•	BreadcrumbList – If your site has breadcrumb navigation, you can add @type: "BreadcrumbList" on pages (like blog posts deep in a hierarchy). Provide itemListElement as an array of ListItem objects, each with a position, name, and an item (the URL) ￼. This helps Google display breadcrumbs instead of full URLs in search results. Ensure the breadcrumb reflects the site structure (e.g. Home > Blog > Post Title). It’s generally placed in the <script> on pages where breadcrumbs are visible to users. Google requires at least two ListItem elements (to form a path) for breadcrumb rich results.
	•	Others (as needed): Sitelinks Search Box is covered via WebSite/SearchAction as noted. If your SaaS has other content types, consider schema like FAQ (as above), HowTo (if you publish guides), JobPosting (if you list jobs on a careers page), etc. But the above are the primary ones for a typical SaaS marketing site and blog.

Nesting and combining schemas: Wherever possible, nest related schema types within one JSON-LD block to show their relationships. For example, an Article schema can include an author property that is a Person object instead of splitting that into a separate script. A Product schema can include aggregateRating and review objects within it (as shown in Google’s example) ￼ ￼. Google recommends nesting over using multiple separate JSON-LD scripts for closely related info ￼ ￼. The reason is that a single combined object helps Google understand the context (e.g. which product the review is for) clearly. That said, it’s acceptable to have multiple JSON-LD scripts for distinct entities on the same page (for instance, an Organization and a Product, or a BreadcrumbList separate from an Article). Google will parse them either way ￼, but avoid duplicating the same type in separate scripts (don’t provide two Organization objects for the same org, etc., as this can confuse parsers or appear as spam) ￼. A good rule is: one script per logical entity, or one script that nests a primary entity and its sub-entities.

Required vs recommended properties: Schema.org itself defines many properties, but Google’s documentation highlights which ones are required/recommended for rich results ￼ ￼. In summary:
	•	For Product: Required for rich snippets – name and aggregate review/offer data. Google expects either offers, review or aggregateRating (with ratingValue, reviewCount) for product snippets ￼. Recommended – image, description, sku, brand. If you have multiple offers (e.g. variants or plans), you can use the advanced ProductGroup schema, but a simple single Product with one Offer is sufficient for one product page ￼. Always include priceCurrency and price in Offer, and update priceValidUntil if applicable for sales ￼.
	•	For Article: No strict required fields by Google, but recommended – headline, datePublished, author, image ￼. Also publisher (Organization with name and logo) if it’s a news article. Including dateModified is good practice if content is updated. These help with appearance in Top Stories or Google Discover.
	•	For FAQPage: Required – each Question must have a name and an acceptedAnswer.text ￼. Also the container must be FAQPage type with mainEntity array. Google will ignore FAQ markup that doesn’t follow this Q&A pattern or if the answers are not visible to users on page (avoid hidden answers).
	•	For BreadcrumbList: Required – an array of at least two ListItem. Each ListItem needs a position, name, and item (URL) ￼. Make sure position starts at 1 and increments.
	•	For Organization: Google’s Knowledge Panel documentation suggests at least name and logo. Adding url is important, and sameAs for social profiles is recommended to verify your identity. If relevant, contactPoint (for customer support contacts) could be added, but not required for SEO.
	•	For WebSite: Required – name and url. To trigger Sitelinks Search, add a SearchAction with the proper target and query-input – that portion must be exactly as Google specifies (with {search_term_string} and required name=search_term_string). Also include potentialAction only on the homepage schema, not on every page.

Validation: Always test your JSON-LD using Google’s Rich Results Test and Schema.org’s Markup Validator. The Rich Results Test will tell you if your page is eligible for specific rich result types and flag errors/warnings ￼ ￼. Warnings (for missing recommended fields) won’t prevent eligibility but suggest improvements. For example, it might warn if an Article is missing an image. The Schema Markup Validator (validator.schema.org) can check syntax and schema compliance beyond Google’s rich result focus. Additionally, once deployed, use Google Search Console’s Enhancements reports to see if Google indexed your structured data or if any errors occurred ￼. This helps catch issues like unintentionally hidden content (Google requires that content in JSON-LD should match what’s visible on the page – don’t put info in JSON-LD that users can’t see, or it may be considered spammy) ￼. As a final check, use browser dev tools to ensure the <script type="application/ld+json"> is present and contains valid JSON (no trailing commas, etc.). Maintaining unit tests for your structured data templates (if feasible) can catch any JSON formatting errors early.

Multi-schema on one page: It’s common to have multiple schemas on a single page – e.g. a SaaS homepage might have Organization and WebSite; a blog post page might have Article and BreadcrumbList. You can include them either as separate <script> blocks or combine into one array. Both methods work. If separate, just output multiple <script type="application/ld+json"> tags – Google will read them all ￼. If combined, you can wrap multiple objects in an array within one script tag. The key is clarity: ensure that if the schemas relate, they reference each other appropriately (for instance, an Article can have "mainEntityOfPage": {"@type": "WebPage", "@id": "URL"} linking back to a WebPage/URL, and the WebPage could in turn reference the Article as mainEntity). But for simplicity, many implementations just keep them distinct. The only “gotcha” is to avoid conflicting info: e.g. don’t declare two different Organization objects for the same org – use one and be consistent. Also, keep the JSON-LD updated if content changes (for example, if a user review count changes, the structured data should be updated upon rebuild or via ISR).

3. Existing Package Landscape

There are a few popular packages related to SEO and structured data in the Next.js ecosystem. Here’s a comparison of key solutions and why teams often end up creating custom approaches:

Package	Purpose & Features	Pros	Cons
next-seo	SEO utilities for Next (primarily Pages Router). Allows defining meta tags, JSON-LD in a config or component manner.	- Simple API for common tags (title, description, openGraph, etc.)- Widely used in Next.js 12 era (Pages Router).	- Not needed with App Router’s built-in metadata API ￼ (Next.js 13+ aims to replace it).- Doesn’t integrate with streaming SSG/SSR – using it in App Router can lead to tags injected at runtime (which bots might miss).- Lacks the type safety and unified approach of Next’s Metadata API. Many teams migrating to App Router drop next-seo to avoid conflict ￼.
next-sitemap	CLI/Script to generate sitemap.xml (and possibly robots.txt) from your Next routes.	- Automates sitemap creation, including dynamic routes, with minimal config.- Can be run post-build to produce XML files.	- Redundant now that Next 13+ supports file-based app/sitemap.ts generation natively ￼ ￼.- External build step – if routes change often (ISR), sitemap might get outdated unless regenerating.- Limited to sitemap/robots – doesn’t handle meta tags or JSON-LD, so it addresses only part of SEO needs.
schema-dts	TypeScript types for Schema.org definitions (JSON-LD). Provides TS types (interfaces) for schema objects.	- Strong type safety for JSON-LD: you can define e.g. WithContext<Organization> and get autocomplete for properties ￼.- Ensures your structured data objects conform to schema.org shape, reducing errors.- Covers a wide range of schema types (auto-generated from schema.org vocab).	- Learning curve: developers need to understand how to use generic types (e.g. Thing, union types for properties).- Not specific to Next.js, so it doesn’t integrate with Next features directly – it’s just a typing aid. You still manually insert the JSON-LD into a <script> (which is fine).- Some very new schema.org properties might lag if the type library isn’t updated frequently, but generally it’s comprehensive.
next-mdx-remote	Utility to load MDX content from any source (including parsing frontmatter). Often used for blogs with custom data sources.	- Can parse MDX files’ frontmatter (YAML metadata) easily into a JS object ￼. This is useful if authors supply SEO metadata (title, description, etc.) in the MDX frontmatter – you can feed that into Next’s generateMetadata.- Allows MDX content to be sourced from outside the filesystem (databases, headless CMS, etc.), which aligns with a CMS-agnostic approach (you mentioned using Convex, not a flat file CMS).- Generally helpful to render MDX pages without bundling at build time.	- If your content isn’t MDX, this adds unnecessary complexity. In your case, using Convex DB, you likely won’t need MDX parsing (unless you later mix in MDX content).- Adds some overhead: e.g. requires enabling transpile for Turbopack and careful server/client usage (as noted in its docs). Not an all-in-one SEO tool – it’s mostly about content rendering, with frontmatter parsing as a perk.- With App Router, Next.js also offers built-in MDX support via next/mdx, though frontmatter handling may still require custom code.

Why teams roll their own solutions: Many existing packages were built before Next 13’s App Router and SEO enhancements. With the new Metadata API, some older libraries are less necessary or compatible ￼. Teams often craft custom solutions to have full control and type safety. For instance, next-seo was great for Pages Router, but in App Router it’s simpler and safer to use the built-in API (the metadata merging, streaming, etc. are handled by the framework). A custom approach can ensure consistency across pages (via a central config), and integrate tightly with the project’s data sources (e.g. your Convex backend) without being constrained by a library’s abstractions.

Another factor is flexibility: SEO requirements can be very specific per project. Rolling your own allows you to decide exactly how metadata inheritance works, how JSON-LD is generated, and to include project-specific types (like perhaps a custom schema for your AI chatbot if needed) that generic libraries don’t support out of the box.

Finally, performance and maintenance: fewer dependencies means less to update or conflict. Next.js is evolving quickly; some packages may lag behind Next releases. By using the core Next features and a thin custom layer, teams avoid issues when upgrading Next.js. In your case, building an in-house package ensures it can be i18n-ready, framework-agnostic for content, and typed to your needs – filling the gaps left by combining multiple smaller packages.

4. Implementation Patterns

Designing an SEO/metadata helper package for Next.js requires balancing a central configuration with per-page customization:
	•	Centralized default config: It’s best to define your global defaults in one place (e.g. in the root layout.tsx or an internal config file). This includes your site’s name, default description, base URL, default OG image, etc. Next.js allows a root layout export const metadata which children will inherit and shallow-merge from ￼ ￼. For example, set a default title template and description at the root:

// app/layout.tsx (root)
export const metadata: Metadata = {
  title: {
    template: '%s | My SaaS Platform',
    default: 'My SaaS Platform'  // will be used if a page doesn’t set a title
  },
  description: 'An AI-powered platform for ...',
  metadataBase: new URL('https://my-saas.com'), 
  openGraph: {
    type: 'website',
    siteName: 'My SaaS Platform'
  },
  robots: { index: true, follow: true }
};

This provides a baseline. Your package can allow defining this config via a single JSON/TS object. By having it in one place, you ensure consistency (no page forgets the siteName or uses a wrong base URL). Developers using the package should be able to set these global settings easily (perhaps via an init function or passing the config to a provider).

	•	Per-page overrides and merging: Each page or dynamic route can then supply its specific metadata. Next merges metadata shallowly – which is useful. For instance, if a page exports metadata = { title: "Pricing", description: "Plans for ...", openGraph: { images: [ {...} ] } }, it will override the title/description and add the OG image on top of the defaults defined in the layout (e.g. retaining the siteName for OG if not overridden) ￼ ￼. Shallow merge means if a child defines a key, it replaces the parent’s key entirely (except for arrays which concatenate in some cases). If deep merge is needed, you can do it manually (e.g. copy parent values). Your package API should make this easy – perhaps exposing a helper that takes page-specific data and merges it with global defaults (allowing deep merge if needed). A pattern could be:

// pseudo-code usage
import { makeMetadata } from 'next-seo-package';
export async function generateMetadata({ params }) {
  const product = await getProduct(params.id);
  return makeMetadata({
    title: product.name,
    description: product.summary,
    openGraph: {
      images: [ { url: product.ogImageUrl } ]
    }
  });
}

Here, makeMetadata would internally combine the passed object with the global config (knowing to apply metadataBase, default siteName, etc.). This abstracts Next.js’s merging so developers don’t repeat boilerplate.

	•	Absolute URL generation: A frequent source of bugs is constructing full URLs for canonicals or social images. Using Next’s metadataBase as above is the recommended approach ￼ ￼. Ensure that your package either (a) requires the user to provide a base URL (once) or (b) auto-detects it from config. Then, when the user specifies a path like "/blog/my-post", Next will output <link rel="canonical" href="https://my-saas.com/blog/my-post"> ￼. If for some reason you need to manually concatenate URLs (like for JSON-LD links), be careful to avoid double slashes or missing slashes. A utility in your package could safely join base URLs and paths. Also consider multi-domain scenarios (maybe in future if you have locales on different domains) – the package’s config format might allow per-locale base URLs.
	•	Image optimization for OG: Social sharing images (Open Graph/Twitter) should be 1200×630 pixels (the standard for a good large preview) ￼. While Next.js can optimize  elements, OG images are just URLs in meta tags – but you can still leverage Next for hosting/generating them. Two approaches: (1) Static images – you might store these in public/ or an external CDN. Ensure the image file is of correct dimensions and size (Twitter recommends <5MB). (2) Dynamic OG image generation – Next 13+ introduced the opengraph-image.tsx and twitter-image.tsx special routes that can programmatically generate images (using <ImageResponse>). If your SaaS wants to generate images with custom text (like title overlay), your package could integrate that by providing a template or suggestions to create those routes. At minimum, document the file-based approach: e.g. developers can create app/[slug]/opengraph-image.tsx to generate an image for each blog post. If that’s overkill, simply encourage providing an URL to an image. You might also consider integration with Next’s <Image> component for any inline images on pages, but for SEO meta images it doesn’t directly apply except ensuring the source is optimized. In summary, the package should let devs specify an OG image path per page (or a default one globally) and perhaps warn if the image dimensions might be incorrect. Possibly include in your validation checklist to check OG image sizes.
	•	Sitemap and robots.txt: Next.js now lets you define app/robots.txt and app/sitemap.xml routes directly (which can be dynamic) ￼. Your package can streamline this. For example, you could provide a function generateSitemap(entries: SitemapEntry[]) that developers can use in their app/sitemap.ts file (or your package might even export a pre-made sitemap.ts). The same for robots.txt – e.g. allow config of disallow rules and generate the text. Since you have multiple products, a unified sitemap that merges all routes (maybe by crawling the Next route definitions or by accepting an array of URLs from each app) could be helpful. Ensure to include dynamic content: e.g. pulling all blog post slugs from Convex and listing them. The code snippet from the Substack shows how to fetch and map posts in a sitemap route ￼. Also, consider priority and changefreq fields in sitemap: allow customizing them per section (though Google doesn’t heavily rely on those, some teams like to set them). By handling sitemap generation in the package, you free developers from manual XML and ensure it’s always based on current routes/data. For robots.txt, a common pattern is to allow everything (User-agent: * + Disallow: nothing) by default, with an option to disallow certain paths (like admin areas or preview). Provide a simple interface to configure this, and your package can output the correct format.
	•	Default metadata inheritance: It’s worth emphasizing an inheritance hierarchy that matches Next’s layouts. For example, your package might encourage setting site-wide defaults in RootLayout, section defaults in section layouts (e.g. a <BlogLayout> could set metadata = { title: "Blog – MySite", description: "Latest news..." }), and then only specific fields in pages. Next.js will merge from root down to page, where deeper levels override parent values ￼. Document this pattern for users of your package. Possibly provide a debugging tool (maybe in dev mode) to log the final metadata for a page and what defaults were applied, so developers know their config inheritance is correct.
	•	Handling SSR vs SSG: Since your content is a mix of static and dynamic, ensure that any helper functions can run in both scenarios. For instance, if you use generateMetadata, it will run at build time for SSG pages and at request time for SSR/ISR pages. Your package’s API should not assume a Node.js environment beyond what Next provides. (For example, avoid using things like window or document obviously, and be careful with any file I/O). Use Next’s paradigms: if data is needed at build (for SSG), the developer should provide it (maybe via params or by calling their data fetching inside generateMetadata with revalidate as appropriate). Essentially, the package should be mostly pure functions and objects that Next.js can consume during its render process.
	•	Client-side nuances: Notably, App Router’s metadata cannot be updated on the client after navigation – Next.js handles metadata on navigation by re-evaluating it on the server for the new page. This means your package doesn’t need to provide any client-side logic for SEO (no React context or effect to update <head>). However, you might want to integrate with Next’s <Meta> component if it existed – but since we rely on the provided API, it’s better to stick to that. If someone using your package asks “how do I update metadata in a client component?”, the answer is: you generally shouldn’t – design the data flow so that needed info is fetched in generateMetadata (for example, if a page has client-side state, you can’t reflect that in SEO tags until a full page transition or reload). This limitation should be documented so developers structure their app accordingly (for instance, a tabbed interface that swaps content should ideally not be separate SEO pages unless each tab is a distinct route, etc.).

5. Edge Cases & Gotchas

Every SEO implementation has quirks. Watch out for these edge cases in Next.js and metadata:
	•	Trailing slashes & URL consistency: As mentioned, Next.js by default removes trailing slashes from routes (and will redirect /about/ to /about) ￼. Ensure your canonical URLs follow the same format. If your marketing site is currently accessible at both with and without slash, pick one and redirect the other. Next’s trailingSlash setting in next.config.js can force one behavior ￼. In the package, perhaps provide a config flag for trailing slash preference, and use it when constructing canonical links (e.g. avoid accidentally adding a “/” in alternates.canonical if not wanted). A related point: if you ever have URLs with uppercase or mixed-case, normalizing to lowercase in canonical is wise (Next generally treats routes case-insensitively on Windows but case-sensitively on Linux, so stick to lowercase paths).
	•	Preview/Draft mode SEO: Next.js Draft Mode allows content previews that are not meant for indexing ￼. If your SaaS uses preview links (like “see how your unpublished blog will look”), those pages should include <meta name="robots" content="noindex">. Using the robots metadata field is the cleanest approach ￼. Your package could detect a draft mode (Next provides draftMode().isEnabled in server components) ￼ and automatically add noindex/nofollow when true. Alternatively, expose an easy way for devs to mark a page as “noindex” via metadata config. Don’t forget to remove or override that when the content goes live. Also, ensure preview pages still include other necessary metadata (perhaps with “[Draft]” in the title to avoid confusion).
	•	Pagination and duplicate content: If you have paginated listing pages (e.g. /blog?page=2), you may want to handle canonical tags carefully. Google sometimes treats page 1 as canonical for others, but the safer approach is to self-canonicalize each paginated page or use rel="prev"/next" link tags (though Google no longer officially uses those for indexing, some SEO professionals still add them for other bots). Next’s Metadata API does not have built-in prev/next fields, so this would require manually adding via <Head> or a custom component – possibly out of scope for the package unless you choose to support it via a manual step. At least document that paginated pages should have distinct titles (e.g. include “– Page 2” in the title) to avoid complete duplication. For structured data on archive pages, you could consider using an ItemList schema (list of blog posts with their URLs), but that’s optional and not directly tied to rich results – primarily it might help search understand the relationship. It’s an advanced use case: the package might not generate those by default, but could allow hook for it.
	•	Large FAQ or long lists in JSON-LD: If you have an FAQ page with dozens of Q&As, note that extremely large JSON-LD scripts can be a performance issue (lots of kilobytes) and might hit parser limits. Google hasn’t published a size limit, but keep it reasonable. If an FAQ is very long, consider splitting into sections or limit what you mark up (maybe top 10 Qs). Also, if the same question appears on multiple pages, don’t mark it up in multiple places – Google might see it as duplicate FAQ content. Generally, one page = one FAQ schema block covering that page’s FAQs. These are content considerations to mention to future users.
	•	Client-side navigation quirks: When users navigate within a Next.js SPA, Next will seamlessly swap the <head> based on the new page’s metadata. One quirk is that metadata like <title> will update, but if you rely on any runtime check of meta tags (unlikely), it might not fire a DOM event. This usually isn’t an issue, but if someone tries to use a React effect to read or change document.title, it’s better to rely on Next’s managed title. Essentially: do not attempt to manage <head> in client code. Also, be aware that during development or if an error occurs, you might briefly see default metadata before it gets replaced (due to streaming). This is fine for users, but for automated checks maybe pause until the page is fully loaded.
	•	Structured data and rehydration: Adding JSON-LD via the Metadata API isn’t directly supported – you can’t put JSON-LD in the metadata object (it only supports certain fields). The approach is to include a <script> in your page JSX as shown earlier ￼ ￼. This means the script is part of the rendered output. One gotcha: if you use a Client Component for the page and try to render a <script> with dangerouslySetInnerHTML, React might complain or it might not work as expected (since it doesn’t run on server). The solution is to ensure that JSON-LD <script> is rendered in a Server Component (which it will be if your page or layout is server). If you ever have to generate structured data based on client state, you’re out of luck – that should be avoided or done via an API that pre-renders. So basically, no issues as long as structured data is based on server-known data (which it should be for SEO).
	•	International SEO (future-proofing): Even though the initial scope is English-only, it’s smart to design the API with localization in mind. This includes: allowing a list of locales and their base URLs or path prefixes, generating hreflang tags (as discussed), and possibly locale-specific schema. For example, if you later support multiple languages, an Organization schema could include a localized name or you might include inLanguage on things like Article. Google primarily relies on hreflang links for locale alternate pages, not on schema, so hreflang is priority. Ensure your package can either automatically add hreflang for each locale (if you provide the locales and route structure) or at least not make it hard. Also consider the x-default hreflang for a default language – Next’s alternates API supports "x-default": "/en-us/page" to indicate the default locale ￼. Document how to set that in your config when the time comes. Lastly, if using subdomains or separate domains for locales, your package config should accommodate a mapping of locale to domain.

In summary, careful handling of these edge cases will make the SEO package robust. Many of these (canonical consistency, draft noindex, hreflang) can be handled at the package level by offering sensible defaults (e.g. auto-noindex previews) and clear documentation for developers.

⸻

Best Practices Summary (for Package API Design)
	•	Leverage Next’s native APIs: Use the App Router Metadata API exclusively for setting meta tags ￼. Avoid client-side head manipulation or mixing redundant libraries ￼. The package should wrap Next’s metadata/generateMetadata usage, not reinvent it.
	•	Global config with overrides: Provide a single SEO config object (site name, base URL, defaults) that applies globally, with easy per-page override hooks. Default metadata (title template, default description) should be defined once and inherited by all pages unless changed ￼.
	•	Ensure absolute URLs: Require or infer a metadataBase (e.g. from an env var like NEXT_PUBLIC_SITE_URL) so that all relative URLs (canonical links, OG images) are output as fully qualified URLs ￼ ￼. This prevents mistakes in link tags.
	•	Automate social and canonical tags: Include canonical URL generation for each page (perhaps by default using the current route). Simplify multi-language hreflang generation via config (e.g. if locales provided, package outputs alternates.languages accordingly). Include Open Graph and Twitter card fields in the API and auto-fill them from defaults if not provided (e.g. use the page title/description for OG by default to avoid duplication).
	•	Integrate structured data templates: Offer reusable JSON-LD templates for common types (Organization, Product, Article, FAQ, Breadcrumb) to encourage usage. These could be functions that take inputs (e.g. an Article function that accepts title, author, date, etc., and returns a JS object or JSON string). This ensures schema is properly formatted and easy to include.
	•	Type safety and IntelliSense: Use TypeScript types for the SEO config and for JSON-LD helpers (possibly using schema-dts under the hood for JSON-LD types). This gives developers autocompletion (e.g. they can see allowed properties for openGraph or for an OrganizationSchema). Type safety reduces runtime errors in meta tags.
	•	No double data fetching: Document patterns (or provide utilities) to avoid fetching the same data twice for page content and SEO. For instance, allow passing data into the SEO functions if it’s already loaded, or use Next’s caching as shown in examples ￼ ￼.
	•	Flexible for any data source: The package should accept data as parameters, not fetch it internally. This way it works with Convex, CMS, markdown, etc. For example, a generateArticleMetadata(post: Post) function could take a post object (regardless of origin) and return the right meta and JSON-LD.
	•	Validation and warnings: Possibly include dev-time warnings for common SEO mistakes. For instance, if a page’s metadata is missing a canonical or has a very short description, console.warn about it. Or if the same title is used on multiple pages (if detectable). These checks can help maintain quality. Also consider a build-time script or test utility that scans generated pages for SEO issues (optional, but a nice touch).
	•	Great developer experience: Aim for a DX where adding SEO is not tedious. Perhaps a high-level component or hook (though not for head, but maybe for JSON-LD). For example, <JsonLd schema={ProductSchema(product)} /> that automatically inserts the script tag. Or a simple usage in generateMetadata: return SeoMetadata({ title: ..., description: ..., schema: [ArticleSchema(post), BreadcrumbSchema(pathArray)] }). The idea is to make the right thing the easy thing.

Schema Templates

Below are JSON-LD schema templates for common SaaS pages. These are provided as examples (in practice, you’d output them via a <script> tag in your Next.js page):

1. Homepage (Organization + WebSite) – Include your Organization info and WebSite with search action:

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Your SaaS Inc.",
  "url": "https://your-saas.com/",
  "logo": "https://your-saas.com/static/logo.png",
  "sameAs": [
    "https://www.linkedin.com/your-company",
    "https://twitter.com/yourcompany"
  ]
}
</script>

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "url": "https://your-saas.com/",
  "name": "Your SaaS Platform",
  "potentialAction": {
    "@type": "SearchAction",
    "target": "https://your-saas.com/search?q={search_term_string}",
    "query-input": "required name=search_term_string"
  }
}
</script>

(The Organization schema defines the company; the WebSite schema enables the search box in Google results. “sameAs” links help connect social profiles to your organization ￼.)

2. Product/Pricing Page (SoftwareApplication or Product) – Describe the SaaS product and pricing offer:

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Awesome AI SaaS",
  "description": "Awesome AI SaaS is a platform that ... (brief marketing copy)",
  "image": "https://your-saas.com/assets/product-screenshot.png",
  "brand": {
    "@type": "Organization",
    "name": "Your SaaS Inc."
  },
  "offers": {
    "@type": "Offer",
    "price": "49.00",
    "priceCurrency": "USD",
    "availability": "https://schema.org/InStock",
    "url": "https://your-saas.com/signup"
  }
}
</script>

(This marks up the product. If you had multiple plans, you could list multiple Offer objects or use additional property like "priceCurrency": "USD" and different price for each, but a single representative offer is shown. Include aggregateRating and review if you have them – e.g. aggregateRating with ratingValue and reviewCount to show stars ￼ ￼.)

Alternatively, if using SoftwareApplication schema:

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "Awesome AI SaaS",
  "applicationCategory": "BusinessApplication",
  "operatingSystem": "All (Web)",
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.8",
    "ratingCount": "250"
  },
  "offers": {
    "@type": "Offer",
    "price": "49.00",
    "priceCurrency": "USD",
    "url": "https://your-saas.com/pricing"
  }
}
</script>

(SoftwareApplication might be used if you want to emphasize it’s a software product. It can coexist with Product schema, but often one or the other is fine. The presence of aggregateRating with high values can make star ratings eligible to show up.)

3. Blog Post Page (Article) – Mark up a blog article:

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://your-saas.com/blog/how-to-use-our-product"
  },
  "headline": "How to Use Our Product Effectively",
  "image": "https://your-saas.com/blog/how-to-use-our-product/cover.jpg",
  "author": {
    "@type": "Person",
    "name": "Jane Doe"
  },
  "datePublished": "2025-08-01T12:00:00Z",
  "dateModified": "2025-08-01T12:00:00Z",
  "publisher": {
    "@type": "Organization",
    "name": "Your SaaS Inc.",
    "logo": {
      "@type": "ImageObject",
      "url": "https://your-saas.com/static/logo.png"
    }
  },
  "description": "A brief summary of the blog post for SEO purposes."
}
</script>

(This is using BlogPosting (which is a subtype of Article). The mainEntityOfPage points to the page URL – good practice to link the Article to the page. Include an image if possible (Google likes at least one image of 1200px for articles). The description is optional but can reinforce the snippet. The publisher Organization with logo is more relevant for Google News/AMP, but it’s a nice completeness.)

4. FAQ Page (FAQPage) – Mark up an FAQ section:

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [{
      "@type": "Question",
      "name": "What is Awesome AI SaaS?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Awesome AI SaaS is a platform that allows you to ... (answer content)."
      }
    },
    {
      "@type": "Question",
      "name": "How much does it cost?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "We offer a free tier, and premium plans starting at $49/month. (etc)..."
      }
    }
    <!-- more questions as needed -->
  ]
}
</script>

(Each Q&A pair becomes a Question with an acceptedAnswer. Ensure the question text matches exactly what’s on the page, and the answer text as well. Don’t include too much HTML in the text – simple paragraphs or lists are fine, but if you need formatting, you can include basic HTML tags inside the string.)

5. About/Team Page – Mark up the company info and possibly team members:

For an About page, you can reuse the Organization schema with additional details, and if you have a list of team members, include them as Persons related to the org:

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Your SaaS Inc.",
  "url": "https://your-saas.com",
  "foundingDate": "2020-01-15",
  "founders": [{
    "@type": "Person",
    "name": "Jane Doe"
  }],
  "employee": [
    {
      "@type": "Person",
      "name": "John Smith",
      "jobTitle": "CEO"
    },
    {
      "@type": "Person",
      "name": "Alice Johnson",
      "jobTitle": "CTO"
    }
  ],
  "sameAs": [
    "https://www.crunchbase.com/organization/your-saas",
    "https://www.linkedin.com/company/your-saas"
  ]
}
</script>

(This shows an Organization with founder and employee list. Google may not use this directly in search results, but it contributes to the Knowledge Graph data about your company. If privacy is a concern, listing every employee might not be desired – often just founders or key persons are listed. You could also use a simpler approach: just Organization with description of company, and perhaps no Persons. There is also an "AboutPage" type you could theoretically use with mainEntity pointing to the Organization, but that’s not commonly seen. The above is sufficient.)

These templates can be adapted to your data. The idea for the package is to provide functions or JSON builders for each: e.g. organizationSchema({...}) that returns the JS object so the developer can insert it. That ensures consistency (same structure) and easier maintenance.

API Design Recommendations

Considering the above, the ideal developer experience with your SEO package might look like:
	•	Simple setup: Developer installs the package and in their RootLayout (or _app equivalent) does something like:

import { SeoProvider, createSeoConfig } from 'my-seo-package';

export const metadata = createSeoConfig({
  siteName: "Your SaaS Inc.",
  titleTemplate: "%s | Your SaaS",
  defaultTitle: "Your SaaS – AI Platform",
  description: "Default meta description...",
  baseUrl: "https://your-saas.com",
  defaultSocialImage: "/static/default-og.png",
  twitterHandle: "@YourSaas"
});

This createSeoConfig would produce a Metadata object applying those defaults to all pages. Perhaps the package provides a <SeoProvider> to wrap the app (though since everything is server-side, a provider might not be necessary, unless you want to use context for something else). But a simple function call to generate the root metadata is nice.

	•	Page-level usage: On a page or layout, the developer should be able to specify page-specific SEO without retyping everything. For instance, perhaps:

import { makePageMetadata, ArticleSchema } from 'my-seo-package';

export async function generateMetadata({ params }) {
  const post = await getPost(params.slug);
  return makePageMetadata({
    title: post.title,
    description: post.excerpt,
    alternates: { canonical: `/blog/${post.slug}` },
    openGraph: {
      // images could default to something but here we override
      images: [ { url: post.coverImageUrl } ]
    }
  });
}

Here makePageMetadata would merge the passed object with the global config (so it automatically adds siteName to title template, etc.). This abstracts the Next.js merging details from the user. It could also auto-handle things like adding the canonical if not provided (maybe default to current path).
Additionally, if the package knows the page is an Article (perhaps by context or an option), it could even inject structured data. But better to keep structured data separate to avoid magic.

	•	Structured data helpers: The package should provide functions or components for JSON-LD. For example, an ArticleSchema(post) function that returns the JSON object for that post’s schema. The developer can then do:

import { JsonLd } from 'my-seo-package';

export default function BlogPostPage({ post }) {
  // ... page content ...
  return (
    <>
      <ArticleContent ... />
      <JsonLd data={ ArticleSchema(post) } />
    </>
  );
}

The <JsonLd> component would simply render a <script type="application/ld+json"> with the JSON stringified. This saves the developer from writing the dangerouslySetInnerHTML every time. It also could take an array of schemas or multiple calls for multiple scripts. (Alternatively, if not using a component, you could instruct to use something like {... schema object ...} in the page directly, but a component is cleaner).

	•	Sitemap integration: Provide a straightforward API for sitemap. e.g.:

// app/sitemap.ts
import { getSitemapEntries, SitemapEntry } from 'my-seo-package';
import { allBlogSlugs } from '@/lib/api';

export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const staticEntries = getSitemapEntries(); // maybe reads from config: static routes like '/', '/about', etc.
  const posts = await allBlogSlugs();
  const blogEntries: SitemapEntry[] = posts.map(slug => ({
    url: `/blog/${slug}`,
    lastModified: new Date().toISOString()
  }));
  return [...staticEntries, ...blogEntries];
}

The package could allow defining static routes in the global config (with priority/changefreq if needed) and provide a helper to format them. MetadataRoute.Sitemap is the type Next expects, so ensure compatibility. The idea is to reduce boilerplate in assembling sitemaps.

	•	Robots.txt generation: Similarly, maybe:

// app/robots.ts
import { makeRobotsTxt } from 'my-seo-package';

export default function robots() {
  return makeRobotsTxt({
    allow: ['/'], 
    disallow: ['/admin', '/drafts'],
    sitemapUrl: 'https://your-saas.com/sitemap.xml'
  });
}

which returns a Response with the text. (Next’s robots.ts can export either an object or text – you’d return a response with content-type text/plain).

	•	Built-in defaults and sanity: The package should handle edge defaults like automatically adding a trailing slash in canonical if baseUrl has it, or ensuring meta descriptions don’t exceed some length (maybe warn if > 160 chars, though Google doesn’t have a hard limit, it truncates ~160). Possibly include a check to remove HTML tags from descriptions (sometimes people accidentally put markup in what should be a plain text meta description).
	•	Documentation & Examples: To make DX great, provide examples (somewhat like above) for common pages. Developers can copy-paste an Article example and just fill in their data. If using TypeScript, the types from your package should guide them to provide the right fields.
	•	Flexibility for future: Keep the API flexible enough to extend. For instance, if Google introduces a new meta tag or a new JSON-LD type, the package should allow adding those without major refactoring. Perhaps allow a generic passthrough for uncommon meta tags (like if someone wants to add <meta name="foo" content="bar">, your API could have an escape hatch, though not strictly necessary as they could still use Next’s head if really needed). But covering the major ones (title, desc, canonical, social, robots, keywords) is likely enough.

In summary, the ideal API makes the common tasks one-liners and enforces best practices by design. A developer using it should hardly have to think about SEO nuances – they just provide content, and your package outputs correct tags and schema.

Validation Checklist

To verify that the SEO implementation is correct and complete, use this checklist:
	•	Meta Tags Verification:
	1.	Unique Title & Description: Check that each page produces a unique <title> and <meta name="description"> that matches the content. No placeholder or duplicate titles across pages ￼.
	2.	Title Template Applied: Ensure the site name or title template is correctly appended or prepended (e.g. “Page Title | My SaaS”). In dev tools, look at the <title> tag for a subpage – does it follow the template?
	3.	Canonical Link: Verify a single <link rel="canonical" ...> exists and points to the correct URL (and uses https, correct domain, no unintended // issues). Navigate to a couple of pages and view-source to see the canonical.
	4.	hreflang Links (if applicable): If multiple languages are configured, check that each page’s source has the appropriate <link rel="alternate" hreflang="xx" ...> tags including an x-default if used ￼.
	5.	Robots Meta: Ensure pages that should be indexed have <meta name="robots" content="index,follow"> (or no meta at all, which defaults to index). And pages that should not (like previews or sensitive pages) have noindex set ￼. Use Search Console’s URL Inspection on a noindexed page to confirm Google sees the noindex.
	6.	Social Meta (OG/Twitter): For a representative page, use the Facebook Sharing Debugger and Twitter Card Validator to fetch the URL. Confirm that:
	•	OG tags (og:title, og:description, og:image, og:url) are present and correct.
	•	Twitter tags (twitter:card, twitter:title, etc.) are present. If using summary_large_image, check the image appears. (Remember images must be accessible – if using a dynamic route for images, ensure it deploys correctly.)
	•	Image dimensions and sizes are adequate (e.g. the debugger will show a preview or errors if image is too small).
	7.	No Duplicate Tags: Ensure that there aren’t multiple <title> tags or multiple canonical tags. Next.js should avoid that by design (merging process), but if using a custom Head anywhere, duplicates could slip in. View source for anomalies.
	•	Structured Data Validation:
	1.	Rich Results Test: Run key pages (homepage, a product page, a blog page, FAQ page) through Google’s Rich Results Test ￼. Check that each intended schema is detected and has no errors. For example, the blog post should show an Article item with all required fields green. Fix any errors (the tool will list missing required fields or syntax issues).
	2.	Schema Markup Validator: For a thorough check, use the generic schema validator (validator.schema.org) on the JSON-LD code. This can catch JSON format issues or misuse of types.
	3.	Multiple Schema Coordination: If multiple schemas are on one page, ensure they each are valid. Also verify that they are not contradictory. E.g. if you have both Product and SoftwareApplication on the same page describing the same thing, it might confuse Google – prefer one or nest them. If both are present intentionally (maybe your homepage has Organization and Website – that’s fine as they’re different things), just check both appear.
	4.	Review Stars (if applicable): If you included aggregateRating on Product or reviews on Article, check Google’s feedback in Rich Results Test. Sometimes Google will warn if, say, an article has a rating but the page might not qualify (they have guidelines on which types can have ratings). Ensure your usage aligns with Google’s schema guidelines (for example, Product reviews are fine, but Article shouldn’t have product review unless it is a review article).
	5.	Breadcrumb test: If BreadcrumbList is added, see that Google’s tool detects a Breadcrumb and no errors like missing item or name. Also test the live page by searching for it in Google (once indexed) to see if breadcrumbs show in the snippet.
	•	Performance and Indexing:
	1.	Lighthouse SEO Audit: Run a Lighthouse audit (in Chrome DevTools, under “Lighthouse” -> SEO). It will flag issues like missing descriptions, tap targets, etc. This is a quick sanity check – a score of 100 indicates you have all basic tags.
	2.	Coverage in Search Console: After deployment, monitor Google Search Console. Check the Coverage and Page indexing reports – see if any pages are unexpectedly listed as Excluded due to “Alternate page with canonical tag” or “Duplicate without user-selected canonical”. If so, it might indicate canonical issues (like two pages pointing to each other or something). Ideally, each page you want indexed is Submitted and Indexed, and duplicates are minimal.
	3.	Search Appearance: In Search Console, look at Enhancements (FAQ, Breadcrumbs, etc., if you added those). Ensure those reports show your pages and zero errors. For instance, if you see an FAQ enhancement with errors, drill down to fix markup.
	4.	Manual SERP checks: Do a Google search for your site’s key pages (like site:your-saas.com Pricing). Check if:
	•	The titles and descriptions in SERPs match your intended meta (or at least are reasonably drawn from them – Google sometimes rewrites descriptions, but it should be using your provided one if relevant).
	•	Sitelinks appear for your site’s homepage (if your site has multiple important pages, Google might show sitelinks automatically – having clear titles and a good site structure helps).
	•	For FAQ pages, search a question from the FAQ – see if a rich FAQ snippet appears under your result.
	•	For blog posts, if you search the exact title, do you see maybe a rich result with date/author (common for articles) or even a carousel if you have multiple?
	5.	Social Preview check: Aside from validators, actually share a URL on a platform (Slack, Twitter, LinkedIn) to see how the unfurl looks. This can catch things like missing OG image or wrong domain in the URL.
	•	Robots.txt and Sitemap:
	1.	Retrieve /robots.txt and ensure it’s correct (no disallowing something unintentionally, lists the sitemap URL, etc.).
	2.	Retrieve /sitemap.xml (or multiple sitemaps if you generate). Validate that all important URLs are present and the lastmod dates make sense. Submit the sitemap in Google Search Console and ensure it’s fetched successfully. This will also show if any URL in the sitemap is blocked or not indexed (which might indicate an issue).
	3.	If using canonical tags heavily (like cross-domain or cross-environment), ensure no dev/test URLs are referenced. This can happen if the base URL wasn’t set correctly in config and still points to localhost – a critical check before production.

By following this checklist, you can be confident the SEO metadata and structured data implementation is solid. Each item helps catch a specific category of error (from missing tags to schema syntax to unintended noindex). Incorporating automated tests or CI checks for some of these (like Lighthouse or running the package’s own validators) can ensure ongoing quality as well.

Package Comparison Table

(For reference, a summary of existing solutions and how our approach fills the gaps:)
	•	Next SEO: Great for legacy Next.js but not needed in App Router – our package uses Next’s native metadata system, avoiding the complexity of an extra layer ￼. This means better streaming support and no duplication of config.
	•	Next Sitemap: Automates XML generation but our approach is to integrate sitemap logic into the framework (leveraging app/sitemap.ts) ￼. We provide helpers to easily list URLs, making external libs unnecessary.
	•	Schema-dts: Provides types for JSON-LD which we embrace to ensure our schema functions are type-safe ￼. Our package basically stands on the shoulders of schema-dts for correctness, but offers higher-level schema builders tailored to SaaS needs (so you don’t need to know the entire schema.org spec).
	•	next-mdx-remote: Useful if MDX is used – our package is agnostic to content source; if using MDX, one can combine it with next-mdx-remote to feed frontmatter into our SEO config. Essentially, our package doesn’t replace next-mdx-remote but complements it: e.g. you’d parse MDX frontmatter (title, etc.) and then call our makePageMetadata() with those values. If not using MDX, no need for it. We note that many teams still use frontmatter with MDX to manage blog SEO, so we ensure compatibility (for example, our types can align with typical frontmatter fields like title, description, tags).

In conclusion, our approach aims to unify what these tools do well into one consistent developer experience. Instead of stitching together multiple packages, you configure once and get standardized SEO tags and JSON-LD across your SaaS products. It’s about making SEO easy, consistent, and foolproof for Next.js App Router projects.
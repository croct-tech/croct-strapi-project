<p align="center">
  <img src="LaunchPad.jpg" alt="Strapi LaunchPad + Croct" width="480" />
</p>

<h3 align="center">strapi demo with personalization and AB testing</h3>

<p align="center">
  The official <a href="https://github.com/strapi/LaunchPad">Strapi LaunchPad</a> demo powered by <a href="https://strapi.io">Strapi</a> for content management<br/>and <a href="https://croct.com?utm_medium=repo&utm_source=github&utm_campaign=20260000.CO.DE.strapi_nextjs">Croct</a> for real-time personalization and AB testing.
</p>

---

## Background

[LaunchPad](https://github.com/strapi/LaunchPad) is Strapi's official demo: a Strapi project with content types and data ready to go, plus a Next.js client that renders pages composed of dynamic zones managed in the CMS.

This project is a fork of LaunchPad that adds [Croct](https://croct.com?utm_medium=repo&utm_source=github&utm_campaign=20260000.CO.DE.strapi_nextjs) on top. With Croct, any dynamic zone component already managed in Strapi becomes dynamic and ready for AB testing and personalization, without changing how you structure or deliver your content. It's the fastest way to add AB testing and personalization on top of Strapi.

## What is Croct?

Croct is a headless platform for conversion optimization that pairs nicely with strapi. Using Croct, you get:

* [Server-side AB testing](https://croct.com/ab-testing?utm_medium=repo&utm_source=github&utm_campaign=20260000.CO.DE.strapi_nextjs) support with real-time [audience segmentation](https://croct.com/segmentation?utm_medium=repo&utm_source=github&utm_campaign=20260000.CO.DE.strapi_nextjs)
* [Server-side content personalization](https://croct.com/personalization?utm_medium=repo&utm_source=github&utm_campaign=20260000.CO.DE.strapi_nextjs) based on location, behavior, or custom rules
* [Visitor profile explorer](https://croct.com/user-profiles?utm_medium=repo&utm_source=github&utm_campaign=20260000.CO.DE.strapi_nextjs) so you can dive deeper and analyze the user journey using out-of-the-box events.
* Built-in analytics and Bayesian analysis for every variant and experience
* Seamless compatibility with your existing strapi schemas
* Fast implementation with zero CMS migration

With Croct, any content already managed in strapi becomes personalizable and ready for AB testing, without changing how you structure or deliver it.

Unlike currently available plugins, you can run experiments at the component level rather than the field level. Croct replaces static module content with dynamic content, allowing you to manage everything directly on the UI while using strapi content as a [fallback](https://docs.croct.com/reference/sdk/nextjs/content-rendering#fault-tolerance?utm_medium=repo&utm_source=github&utm_campaign=20260000.CO.DE.strapi_nextjs).

Since it comes with built-in audience segmentation and analytics, there's no need to add extra integrations with CDPs to segment visitors or with analytics tools to gather insights.

## What is inside this demo?

- **A Strapi project** with content types and demo data already onboard
- **A Next.js client** that fetches and renders the content from Strapi
- **Croct integration** that makes dynamic zone components personalizable: content editors map any Strapi component to a Croct slot, and Croct takes over delivering the right variant to each visitor, falling back to the original Strapi content when no personalization applies

## How to get started?

Follow these steps to run the project locally.

### Prerequisites

- Node.js 18 to 22 (Strapi does not support newer versions)
- A [Croct](https://app.croct.com?utm_medium=repo&utm_source=github&utm_campaign=20260000.CO.DE.strapi_nextjs) workspace

### 1. Clone and install

```bash
git clone https://github.com/croct-tech/croct-strapi-project.git
cd croct-strapi-project
yarn setup
```

This installs the dependencies of both apps and creates the `.env` files from the `.env.example` templates.

### 2. Set up Croct

Run the Croct CLI in the `next` directory to configure the application ID and API key automatically:

```bash
cd next
npx croct init
```

### 3. Seed Strapi

Import the demo content into Strapi:

```bash
yarn seed
```

### 4. Run

Start Strapi and Next.js together:

```bash
yarn dev
```

Open [http://localhost:3000](http://localhost:3000) for the website and [http://localhost:1337/admin](http://localhost:1337/admin) for the Strapi admin panel.

## How does the integration work?

Adding Croct to an existing Strapi + Next.js project takes three small changes, with no restructuring of your content or pages required. Check the [diff against LaunchPad](https://github.com/strapi/LaunchPad/compare/main...croct-tech:croct-strapi-project:main) to see the exact changes.

**1. Wrap the middleware with `withCroct`**

```ts
// next/middleware.ts
import {withCroct} from '@croct/plug-next/middleware';

export const middleware = withCroct({
    matcher: config.matcher,
    next: request => {
        // Your existing middleware logic (e.g. locale redirects)
    },
});
```

**2. Add the `<CroctProvider>`**

```tsx
// next/app/layout.tsx
import {CroctProvider} from '@croct/plug-next/CroctProvider';

<CroctProvider>
    {children}
</CroctProvider>
```

**3. Map dynamic zones to Croct slots**

A single Croct slot holds a map from Strapi dynamic zone components to Croct slots. When rendering a page, each dynamic zone component with a mapping gets its content from Croct, using the original Strapi content as fallback:

```tsx
// next/app/[locale]/(marketing)/page.tsx
const {content} = await fetchContent(mapping.slot, {
    preferredLocale: params.locale,
    fallback: zone,
});
```

That's it. Once integrated, any component managed in Strapi becomes personalizable and ready for A/B testing through Croct. Mapping a new component to a slot requires no code changes.

## Docs

For more details on the tools used in this project, refer to the official documentation:

- [Croct docs](https://docs.croct.com?utm_medium=repo&utm_source=github&utm_campaign=20260000.CO.DE.strapi_nextjs)
- [Croct Next.js SDK](https://docs.croct.com/reference/sdk/nextjs/installation?utm_medium=repo&utm_source=github&utm_campaign=20260000.CO.DE.strapi_nextjs)
- [Strapi docs](https://docs.strapi.io)
- [Next.js docs](https://nextjs.org/docs)
<p align="center">
  <a href="https://kakikomu.vercel.app">
    <img alt="Example article page" src="https://user-images.githubusercontent.com/552829/160132094-12875e09-41ec-450a-80fc-ae8cd488129d.jpg" width="689">
  </a>
</p>

# Kakikomu (書き込む)	“To write in” — like journaling Next.JS notion CMS


## Intro


It uses Notion as a CMS, [react-notion-x](https://github.com/NotionX/react-notion-x), [Next.js](https://nextjs.org/), and [Vercel](https://vercel.com).

## Features

- Setup only takes a few minutes ([single config file](./site.config.ts)) 💪
- Robust support for Notion content via [react-notion-x](https://github.com/NotionX/react-notion-x)
- Built using Next.js, TS, and React
- Excellent page speeds
- Smooth image previews
- Automatic social images
- Automatic pretty URLs
- Automatic table of contents
- Full support for dark mode
- Quick search via CMD+K / CMD+P
- Responsive for different devices
- Optimized for Next.js and Vercel

## Demos

- [Default demo](https://kakikomu.vercel.app) - Deployed from the `main` branch

## Setup

**All config is defined in [site.config.ts](./site.config.ts).**

This project requires a recent version of Node.js (we recommend >= 16).

1. Fork / clone this repo
2. Change a few values in [site.config.ts](./site.config.ts)
3. `npm install`
4. `npm run dev` to test locally
5. `npm run deploy` to deploy to vercel 💪
6. Double check your [Vercel project settings](#vercel-configuration)

I tried to make configuration as easy as possible — All you really need to do to get started is edit `rootNotionPageId`.

### Vercel Configuration


From your Vercel project settings, you'll want to **disable Vercel Authentication** from `Project -> Settings -> Deployment Protection`.

![How to disable Vercel Deployment Protection setting](https://github.com/user-attachments/assets/a1eb5a1f-da7a-497e-b4f6-f7e851a6cd8a 'How to disable Vercel Deployment Protection setting which causes social media preview image endpoint to return 401 Unauthorized')

💡 If you forget to do this your site will return `401 Unauthorized` responses when crawlers are trying to retrieve the images.

## Preview Images

<p align="center">
  <img alt="Example preview image" src="https://user-images.githubusercontent.com/552829/160142320-35343317-aa9e-4710-bcf7-67e5cdec586d.gif" width="458">
</p>

We use [next/image](https://nextjs.org/docs/api-reference/next/image) to serve images efficiently, with preview images optionally generated via [lqip-modern](https://github.com/transitive-bullshit/lqip-modern). This gives us extremely optimized image support for sexy smooth images.

Preview images are **enabled by default**, but they can be slow to generate, so if you want to disable them, set `isPreviewImageSupportEnabled` to `false` in `site.config.ts`.

### Redis

If you want to cache generated preview images to speed up subsequent builds, you'll need to first set up an external [Redis](https://redis.io) data store. To enable redis caching, set `isRedisEnabled` to `true` in `site.config.ts` and then set `REDIS_HOST` and `REDIS_PASSWORD` environment variables to point to your redis instance.

You can do this locally by adding a `.env` file:

```bash
REDIS_HOST='TODO'
REDIS_PASSWORD='TODO'
```

If you're not sure which Redis provider to use, we recommend [Redis Labs](https://redis.com), which provides a free plan.

Note that preview images and redis caching are both optional features. If you’d rather not deal with them, just disable them in your site config.

## Styles

All CSS styles that customize Notion content are located in [styles/notion.css](./styles/notion.css). They mainly target global CSS classes exported by react-notion-x [styles.css](https://github.com/NotionX/react-notion-x/blob/master/packages/react-notion-x/src/styles.css).

Every notion block gets its own unique classname, so you can target individual blocks like this:

```css
.notion-block-260baa77f1e1428b97fb14ac99c7c385 {
  display: none;
}
```


## Automatic Table of Contents

<p align="center">
  <img alt="Smooth ToC Scrollspy" src="https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fcb2df62d-9028-440b-964b-117711450921%2Ftoc2.gif?table=block&id=d7e9951b-289c-4ff2-8b82-b0a61fe260b1&cache=v2" width="240">
</p>

By default, every article page will have a table of contents displayed as an `aside` on desktop. It uses **scrollspy** logic to automatically update the current section as the user scrolls through your document, and makes it really easy to jump between different sections.

If a page has less than `minTableOfContentsItems` (default 3), the table of contents will be hidden. It is also hidden on the index page and if the browser window is too small.

This table of contents uses the same logic that Notion uses for its built-in Table of Contents block (see [getPageTableOfContents](https://github.com/NotionX/react-notion-x/blob/master/packages/notion-utils/src/get-page-table-of-contents.ts) for the underlying logic).

## Analytics

Analytics are an optional feature that are easy to enable if you want.

### Fathom Analytics

[Fathom](https://usefathom.com/ref/42TFOZ) provides a lightweight alternative to Google Analytics.

To enable, just add a `NEXT_PUBLIC_FATHOM_ID` environment variable, which will only be used in production.

### PostHog Analytics

[PostHog](https://posthog.com/) provides a lightweight, **open source** alternative to Google Analytics.

To enable, just add a `NEXT_PUBLIC_POSTHOG_ID` environment variable, which will only be used in production.



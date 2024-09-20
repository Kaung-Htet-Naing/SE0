---
# You can also start simply with 'default'
theme: bricks
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
# background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: SEO
info: |
   ## KHN
   Weekly dev meeting.
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
   persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
layout: image-left
# the image source
image: /what-is-seo.jpg
backgroundSize: 90%
# a custom class name to the content
---

# Search Engine Optimizations Essentials

Best Practices & Techniques for SEO Optimization

Date - Friday, September 20, 2024

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

<div style="overflow: scroll; max-height: 100%">
## Next.js SEO Guide

Boost your Next.js site's search engine visibility with these essential techniques:
<v-clicks >

-  **Site Map XML**  
   Generate a dynamic sitemap to help search engines index your content efficiently.

-  **Robot.txt**  
   Control which pages and sections of your website are accessible to crawlers.

-  **Static Metadata**  
   Define consistent SEO metadata for static pages to improve ranking and sharing.

   -  **Individual Metadata**  
      Customize metadata for specific pages or sections for targeted optimization.

-  **Dynamic Metadata**  
   Optimize pages with dynamic content by automatically generating metadata for each entry.

   -  **Blog Posts**  
      Ensure every blog post has unique, relevant metadata for better SEO performance.

</v-clicks>

<br><br>

</div>

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/features/slide-scope-style
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->

---

<div style="overflow: scroll; max-height: 100%">
# 🗺️ Setting Up a Sitemap in Next.js

Sitemaps are the easiest way to communicate with Google. They indicate the URLs that belong to your website and when they update so that Google can easily detect new content and crawl your website more efficiently.
A sitemap helps search engines efficiently crawl your site's URLs.

This guide explains how to set up a sitemap in **Next.js** using:

-  **Pages Router**
-  **App Router**

<v-click>
<img src="https://cdn-proxy.slickplan.com/wp-content/uploads/2024/01/2-understanding-sitemaps.png" alt="SEO Diagram" width="400" height="300"/>
</v-click>

</div>

---

<!--
<div class="grid grid-cols-2">
<div></div>
<div></div>
</div> -->

## Pages Router

<br>

### There are two options:

<v-click>
-Manual
</v-click>

<v-click>

```ts
   <!-- public/sitemap.xml -->
   <xml version="1.0" encoding="UTF-8">
   <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
     <url>
       <loc>http://www.example.com/foo</loc>
       <lastmod>2021-06-01</lastmod>
     </url>
   </urlset>
   </xml>

```

</v-click>
<v-click>

<p style="font-size: 12px; color: gray;">Learn more about <a href="https://nextjs.org/learn-pages-router/seo/crawling-and-indexing/xml-sitemaps">sitemap.xml</a> in the official Next.js documentation.</p>
</v-click>

---

<div style="overflow: scroll; max-height: 100%">
-getServerSideProps

<v-click>

```ts
//pages/sitemap.xml.js
const EXTERNAL_DATA_URL = 'https://jsonplaceholder.typicode.com/posts'

function generateSiteMap(posts) {
	return `<?xml version="1.0" encoding="UTF-8"?>
   <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
     <!--We manually set the two URLs we know already-->
     <url>
       <loc>https://jsonplaceholder.typicode.com</loc>
     </url>
     <url>
       <loc>https://jsonplaceholder.typicode.com/guide</loc>
     </url>
     ${posts
			.map(({ id }) => {
				return `
<url>
<loc>${`${EXTERNAL_DATA_URL}/${id}`}</loc>
       </url>
     `
			})
			.join('')}
</urlset>
`
}

function SiteMap() {
	// getServerSideProps will do the heavy lifting
}

export async function getServerSideProps({ res }) {
	// We make an API call to gather the URLs for our site
	const request = await fetch(EXTERNAL_DATA_URL)
	const posts = await request.json()

	// We generate the XML sitemap with the posts data
	const sitemap = generateSiteMap(posts)

	res.setHeader('Content-Type', 'text/xml')
	// we send the XML to the browser
	res.write(sitemap)
	res.end()

	return {
		props: {},
	}
}

export default SiteMap
```

</v-click>
</div>

---

<div style="overflow: scroll; max-height: 100%">
## App Router

<br>

### There are two options:

#### Manual

<v-click>
```xml
//pages/sitemap.ts
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://acme.com</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
    <changefreq>yearly</changefreq>
    <priority>1</priority>
  </url>
  <url>
    <loc>https://acme.com/about</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>https://acme.com/blog</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.5</priority>
  </url>
</urlset>
```

</v-click>
</div>

---

<div style="overflow: scroll; max-height: 100%">

#### Generating a sitemap using code (.js, .ts)

<v-click>

```ts
//pages/sitemap.ts
import type { MetadataRoute } from 'next'

export default function sitemap(): MetadataRoute.Sitemap {
	return [
		{
			url: 'https://acme.com',
			lastModified: new Date(),
			changeFrequency: 'yearly',
			priority: 1,
		},
		{
			url: 'https://acme.com/about',
			lastModified: new Date(),
			changeFrequency: 'monthly',
			priority: 0.8,
		},
		{
			url: 'https://acme.com/blog',
			lastModified: new Date(),
			changeFrequency: 'weekly',
			priority: 0.5,
		},
	]
}
```

</v-click>

</div>

---

<div style="overflow: scroll; max-height: 100%">

# Robot.txt

## What is a robots.txt File?

A robots.txt file tells search engine crawlers which pages or files the crawler can or can't request from your site. The robots.txt file is a web standard file that most good bots consume before requesting anything from a specific domain.

<img src="/example-robots-txt-file.png" alt="asdf Diagram" style="height: 300px;"/>

</div>

---

## Pages Router

<br>

#### Static robots.txt

<br>

<v-click>

```ts
//public/robots.txt

# Block all crawlers for /accounts

User-agent: \*
Disallow: /accounts

# Allow all crawlers

User-agent: \*
Allow: /
Sitemap: https://example.com/sitemap.xml

```

</v-click>

<v-click>
<p style="font-size: 12px; color: gray;">Learn more about <a href="https://nextjs.org/learn-pages-router/seo/crawling-and-indexing/robots-txt">robots.txt</a> in the official Next.js documentation.</p>
</v-click>

---

#### Generate a Robots file

<v-click>

```ts
import type { MetadataRoute } from 'next'

export default function robots(): MetadataRoute.Robots {
	return {
		rules: {
			userAgent: '*',
			allow: '/',
			disallow: '/private/',
		},
		sitemap: 'https://acme.com/sitemap.xml',
	}
}
```

</v-click>

---

<div style="overflow: scroll; max-height: 100%">

# Meta Data

## Page Router

<v-click>

````md magic-move
```ts
import { Html, Head, Main, NextScript } from 'next/document';

export default function Document() {
  return (
    <Html lang='en'>
      <Head>
        <meta charSet='UTF-8' />
        <meta
          name='robots'
          content='index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1'
        />
        <meta property='og:locale' content='en_US' />
        <meta name='author' content='Alamin Shaikh' />
        <meta property='og:image:width' content='920' />
        <meta property='og:image:height' content='470' />
        <meta name='twitter:card' content='summary_large_image' />
      </Head>
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  );
}
```

```ts
import { Html, Head, Main, NextScript } from 'next/document';

export default function Document() {
  return (
    <Html lang='en'>
      <Head>
        {/* Character, robots, and OG image */}
        <meta charSet='UTF-8' />
        <meta
          name='robots'
          content='index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1'
        />
        <meta property='og:locale' content='en_US' />
        <meta name='author' content='Alamin Shaikh' />
        <meta property='og:image:width' content='920' />
        <meta property='og:image:height' content='470' />
        <meta name='twitter:card' content='summary_large_image' />

        {/* Site name and keywords */}
        <meta
          property='og:site_name'
          content='Developing Superior Software for Leading Businesses'
        />
        <meta
          name='keywords'
          content='JavaScript developer, TypeScript developer, Web developer'
        />

      </Head>
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  );
}
```
````

</v-click>

</div>

---

<div style="overflow: scroll; max-height: 100%">

# Meta Data

## App Router

<div class="grid grid-cols-2 gap-x-2">

<v-click>

```ts
import type { Metadata } from 'next'

// either Static metadata
export const metadata: Metadata = {
	title: 'Next.js',
	description: 'The React Framework for the Web',
	url: 'https://nextjs.org',
	siteName: 'Next.js',
	images: [
		{
			url: 'https://nextjs.org/og.png', // Must be an absolute URL
			width: 800,
			height: 600,
		},
		{
			url: 'https://nextjs.org/og-alt.png', // Must be an absolute URL
			width: 1800,
			height: 1600,
			alt: 'My custom alt',
		},
	],
	videos: [
		{
			url: 'https://nextjs.org/video.mp4', // Must be an absolute URL
			width: 800,
			height: 600,
		},
	],
	audio: [
		{
			url: 'https://nextjs.org/audio.mp3', // Must be an absolute URL
		},
	],
	locale: 'en_US',
	type: 'website',
}
```

</v-click>

<v-click>

```ts
// or Dynamic metadata
import type { Metadata } from 'next'

type Props = {
	params: { id: string }
}

export async function generateMetadata({
	params,
	searchParams,
}: Props): Promise<Metadata> {
	// read route params
	const id = params.id

	// fetch data
	const product = await fetch(`https://.../${id}`).then((res) => res.json())

	return {
		title: product.title,
	}
}

export default function Page({ params, searchParams }: Props) {}
```

</v-click>

</div>

<v-click>

```
Good to know:

The metadata object and generateMetadata function exports are only supported in Server Components.
You cannot export both the metadata object and generateMetadata function from the same route segment.

```

</v-click>

<v-click>

<p style="font-size: 12px; color: gray;">

See the <a href="https://nextjs.org/docs/app/api-reference/functions/generate-metadata#metadata-fields" style="">Metadata Fields </a> for a complete list of supported options.

</p>

</v-click>

</div>

---

<iframe src="https://giphy.com/embed/26gsjCZpPolPr3sBy" width="480" height="259" style="display: block; margin: 0 auto;" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

# Thank You!

<p style="font-size: 2rem; text-align: center; animation: fadeIn 2s ease-in-out, pulse 1.5s infinite; font-weight: bold; color: #4EC5D4;">
  Thank You!
</p>

<style>
@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
  100% {
    transform: scale(1);
  }
}
</style>

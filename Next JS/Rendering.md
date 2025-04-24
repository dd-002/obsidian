________

[[Apps Router]] : Refer to this once

> [!NOTE] Disclaimer
> Written keeping app router in mind. Pages router has completely different technique.
> Don't get confused with [[React Rendering]]

## 🧩 Rendering Types in [[Apps Router]] --

1. **Server Side Rendering** -- Site gets rendered on the server (i.e. all apis are called in server and finally html,css,js is sent).
```tsx
// app/page.tsx
export default async function Page() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts?_limit=5', {
    cache: 'no-store', // Ensures fresh data is fetched on each request
  });
  const posts = await res.json();

  return (
    <div>
      <h1>Dynamic Blog (SSR)</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}


```

2. **Client Side Rendering** -- Completely opposite of Server side Rendering
```tsx
// app/page.tsx
'use client';

import { useEffect, useState } from 'react';

export default function Page() {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/posts?_limit=5')
      .then((res) => res.json())
      .then((data) => {
        setPosts(data);
        setLoading(false);
      });
  }, []);

  return (
    <div>
      <h1>Client-Side Blog (CSR)</h1>
      {loading ? (
        <p>Loading...</p>
      ) : (
        <ul>
          {posts.map((post) => (
            <li key={post.id}>{post.title}</li>
          ))}
        </ul>
      )}
    </div>
  );
}
```
3. **Incremental Site Rendering** -- In **ISR**, the page is statically generated at **build time**, but Next.js will **revalidate the page in the background** at a specified interval, making it **partially dynamic**. Here, the page will be **statically generated at build time** and **revalidated** every 60 seconds. If there's a request during the revalidation process, the old cached page is still served until the new one is built in the background.
```tsx
// app/page.tsx
export default async function Page() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts?_limit=5', {
    next: { revalidate: 60 }, // Regenerate every 60 seconds
  });
  const posts = await res.json();

  return (
    <div>
      <h1>Static Blog (ISR)</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}


```

4. **Static Site Generation.** -- Here, Next.js will pre-render the page **at build time**, making it very fast and SEO-friendly. **No `revalidate` or dynamic fetching is involved**.
```tsx
// app/page.tsx
export default async function Page() {
  // Fetch data at build time
  const res = await fetch('https://jsonplaceholder.typicode.com/posts?_limit=5');
  const posts = await res.json();

  return (
    <div>
      <h1>Static Blog (SSG)</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}


```



## Caveats :
1. 🧠 **What is dynamic = "force-dynamic" in Next.js?**
	It's a way to override Next.js' default static optimization and force Server-Side Rendering (SSR) for a specific route or page — even if it looks like it could be statically rendered.
	Explanation : 
		In **Next.js App Router** (`app/` directory), routes are **static by default** if:
		- You don’t use `fetch()` dynamically
	    - There’s no `cookies()`, `headers()`, or `useSearchParams()`
		But sometimes you **do** need dynamic behavior (e.g., reading cookies or request headers), and Next.js might still try to optimize it as static. That’s where:
			`export const dynamic = "force-dynamic";`
2. **Ways of revalidation ?**
     In ISR if we are not using fetch, there are other ways of revalidation as well
		1. **`next: { revalidate: 60 }` (App Router)**
			This syntax is used **in the App Router** (`app/` directory) when working with **Server Components** (or `fetch()` with revalidation). The **`next` object** allows you to specify revalidation intervals for the fetched data directly inside the `fetch()` request.  This is a form of automatic revalidation.
		2. **Using `revalidateTag` for Manual Revalidation (App Router) In the App Router**, If you're using **Server Components**, you might want to manually trigger ISR revalidation without using the `fetch()` function. This can be done with the **`revalidateTag`** method, which allows you to invalidate specific tags associated with your pages. Form of manual revalidation.
		3. **export const revalidate = 60 (App Router)**
		 This is a simpler and more declarative way to specify the revalidation interval for a page or layout in the App Router (app/ directory). It tells Next.js to regenerate the page after the specified number of seconds, and it applies to the entire page or layout. 
		 `export const revalidate = 60 (App Router)`
		 This is a form of automatic revalidation.

## Manual Revalidation --

1. **revalidatePath** :
	**`revalidatePath(path)`** is a Next.js App Router API that lets you **manually revalidate a specific route (URL path)**. It’s useful when you want to **force a specific page to rebuild** (static regeneration) when something changes. It's commonly used in **server actions**, **route handlers**, or **API routes** to **trigger ISR (Incremental Static Regeneration)** for a page.
```tsx
	// app/blog/page.tsx
export const revalidate = 3600; // regenerate every hour, or manually via revalidatePath

export default async function BlogPage() {
  const res = await fetch('https://api.example.com/posts');
  const posts = await res.json();

  return (
    <div>
      <h1>Blog</h1>
      {posts.map(post => (
        <div key={post.id}>{post.title}</div>
      ))}
    </div>
  );
}
```

```tsx
// app/actions.ts (server action)
'use server';

import { revalidatePath } from 'next/cache';

export async function createPost(formData: FormData) {
  const title = formData.get('title');

  // Save the post in your DB or CMS
  await fetch('https://api.example.com/posts', {
    method: 'POST',
    body: JSON.stringify({ title }),
  });

  // Invalidate the blog page
  revalidatePath('/blog');
}
```


2. revalidateTag :
	`revalidateTag()` is a function in **Next.js App Router** that lets you **manually invalidate cached content** that’s associated with a specific **cache tag**. It's part of the **Next.js Cache API**, which gives you **fine-grained control** over how and when your data and pages are revalidated or rebuilt — unlike global page-based `revalidate`.
```tsx
	// app/page.tsx
export default async function Page() {
  const res = await fetch('https://api.example.com/posts', {
    next: { tags: ['posts'] }, // associate this fetch with 'posts' tag
  });
  const posts = await res.json();

  return (
    <div>
      <h1>Blog</h1>
      {posts.map((post) => (
        <div key={post.id}>{post.title}</div>
      ))}
    </div>
  );
}
```

```tsx
// app/actions.ts
'use server';

import { revalidateTag } from 'next/cache';

export async function revalidatePosts() {
  revalidateTag('posts'); // trigger revalidation for all content tagged 'posts'
}
```



______________
The **App Router** is based on the new `app/` directory. It allows you to build **full-stack React apps** with improved:
- Layouts and templates
- Data fetching
- Routing
- Caching (ISR, SSR, CSR)
- Server and client components

```
/app
  ├─ layout.tsx       # Shared layout for this route and its children
  ├─ page.tsx         # Route's main page (e.g., /)
  ├─ loading.tsx      # Optional: loading state while fetching
  ├─ error.tsx        # Optional: error boundary
  ├─ about/
  │   ├─ page.tsx     # Route: /about
  └─ dashboard/
      ├─ layout.tsx   # Layout only for /dashboard and its children
      ├─ page.tsx     # Route: /dashboard

```

**Nested Dynamic Params --**
```
/app
  └── category
      └── [categoryId]
          └── product
              └── [productId]
                  └── page.tsx

```

### ✅ Key Concepts
1. **`page.tsx`** → Defines the actual page
```tsx
// app/page.tsx (route: '/')
export default function HomePage() {
  return <h1>Welcome to the homepage</h1>;
}
```
2. **`layout.tsx`** → Wraps pages with shared layout
```tsx
 // app/layout.tsx
export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <nav>NavBar</nav>
        {children}
        <footer>Footer</footer>
      </body>
    </html>
  );
}
```
3. **Server vs Client Components**
- By default, files in `app/` are **server components**.
- Add `'use client';` to make a file a **client component**.
3. Routing
	Each folder becomes a URL path:
	- `/app/about/page.tsx` → `/about`
	- `/app/blog/[slug]/page.tsx` → Dynamic route `/blog/my-post`
4. **Loading and Error UI**
```tsx
// app/page.tsx
export default async function Page() { /* fetch data */ }

// app/loading.tsx
export default function Loading() {
  return <p>Loading...</p>;
}

// app/error.tsx
export default function ErrorPage({ error }) {
  return <p>Error: {error.message}</p>;
}
```
6. **Dynamic Routing** : [[URl]]
```tsx
// app/blog/[slug]/page.tsx
export const revalidate = 60; // ISR: Revalidate every 60s

export default async function BlogPost({ params }) {
  const res = await fetch(`https://jsonplaceholder.typicode.com/posts/${params.slug}`);
  const post = await res.json();

  return (
    <div className="space-y-4">
      <h2 className="text-xl font-semibold">{post.title}</h2>
      <p>{post.body}</p>
    </div>
  );
}
```

_____________________

#### Navigating to a link:
```tsx
'use client';

import { useRouter } from 'next/navigation';

export default function MyComponent() {
  const router = useRouter();

  const handleClick = () => {
    router.push('/about'); // Navigate to /about
  };

  return <button onClick={handleClick}>Go to About</button>;
}
```

#### Linking between pages:
```tsx
import Link from 'next/link';

export default function Home() {
  return (
    <Link href="/about">
      Go to About Page
    </Link>
  );
}
```

In **Next.js App Router**, both `Link` and `useRouter().push()` (from `next/navigation`) are used to navigate between routes, but they serve slightly different purposes and are used in different contexts.

Use it in the render/JSX to navigate when a user clicks something.
✅ **When to use:**
- For anchor-like navigation (menus, buttons, lists).
- SEO-friendly (renders as an `<a>` tag).
- Preloads the target page in the background (performance boost).

🧭 `useRouter().push()` – For Programmatic Navigation
Use it in event handlers, logic, or side effects when you want to redirect without user clicking a link.
✅ **When to use:**
- Inside buttons, forms, or conditional logic.
- To redirect after submitting a form or finishing some async task.




#### 🧩 `params` vs `searchParams` in Next.js App Router
1. `params` (Dynamic Route Parameters)
These come from the **URL path**, based on your folder structure.
📁 File structure:
```
app/products/[id]/page.tsx
```
📦 Usage in the page:
```tsx
// app/products/[id]/page.tsx

export default function ProductPage({ params }: { params: { id: string } }) {
  return <h1>Product ID: {params.id}</h1>;
}
```

✅ **Example URL:** `/products/123` → `params.id === '123'`

2. `searchParams` (Query String Parameters)
These come from the **?key=value** part of the URL.
📦 Usage:
```tsx
// Example: /search?page=2&term=shoes

export default function SearchPage({
  searchParams,
}: {
  searchParams: { [key: string]: string | string[] | undefined };
}) {
  return (
    <div>
      <p>Page: {searchParams.page}</p>
      <p>Search term: {searchParams.term}</p>
    </div>
  );
}
```

✅ **Example URL:** `/search?page=2&term=shoes`  
→ `searchParams.page === '2'`  
→ `searchParams.term === 'shoes'`

🧠 TL;DR Comparison

| Feature          | `params`                             | `searchParams`                        |
|------------------|--------------------------------------|----------------------------------------|
| Comes from       | The URL path (`[id]`, `[slug]`)      | The query string (`?key=value`)        |
| Defined in       | Folder/file structure                | Appended to the URL manually           |
| Type             | Required for dynamic routes          | Optional, flexible filtering           |
| Example URL      | `/post/42` → `params.id = '42'`      | `/post?id=42` → `searchParams.id = '42'` |





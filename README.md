# Next.js Quick Guide & Notes 🚀

## 1. إنشاء مشروع Next.js جديد

* افتح الترمينال (CMD أو Terminal).
* نفذ الأمر:

```bash
npx create-next-app@latest
```

* بعد ما ينتهي، ادخل على مجلد المشروع:

```bash
cd your-project-name
```

* شغل المشروع:

```bash
npm run dev
# or
yarn dev
```

* الصفحة الافتراضية التي ستراها موجودة في:
  `app/page.js`

---

## 2. تنظيم الملفات (App Folder Structure)

مثال عملي:

```
app/
 ├─ page.js        // الصفحة الرئيسية
 └─ posts/         // فولدر جديد لأي Posts
     └─ page.js    // صفحة لكل البوستات
```

---

## 3. Links in Next.js 🚀

### Why Next.js `Link` is better than React `<a>`:

1. **Client-side routing**:

   * `Link` يمنع إعادة تحميل الصفحة بالكامل (`Full page refresh`).
   * في React التقليدي، لو استخدمت `<a href="/posts">` الصفحة هتعمل Reload كامل.

2. **Prefetching**:

   * Next.js يقوم تلقائيًا بتحميل الصفحة الهدف في الخلفية إذا ظهرت على الشاشة (On hover)، بالتالي التنقل أسرع جدًا.

3. **SEO friendly**:

   * `Link` يحافظ على بنية الـ HTML والـ SEO بشكل أفضل من بعض حلول الـ SPA التقليدية.

---

### Basic Example:

```jsx
import Link from "next/link";

export default function Home() {
  return (
    <div>
      <h1>Welcome to My Blog</h1>
      <Link href="/posts">
        <button>Go to Posts</button>
      </Link>
    </div>
  );
}
```

> ملاحظة: في Next.js 13+، `Link` أصبح يقبل أي children، مش شرط `<a>`، لكن لو عايز تستخدم `<a>` عادي برضه ممكن.

---

### Advanced Example with `passHref` & `<a>`:

```jsx
import Link from "next/link";

export default function Navbar() {
  return (
    <nav>
      <Link href="/posts" passHref>
        <a className="text-blue-500 hover:underline">Posts</a>
      </Link>
    </nav>
  );
}
```

> `passHref` ensures `<a>` gets the `href` prop, useful if you want semantic HTML.

---

### Tips:

* استخدم `Link` لكل internal navigation داخل المشروع.
* لو عندك external link، استعمل `<a>` عادي مع `target="_blank"` و `rel="noopener noreferrer"`.
* دمج Link مع Buttons: Next.js يسمح بيه بدون مشاكل.

---

## 4. Metadata (Title, Description, etc.)

```jsx
export const metadata = {
  title: "My Awesome Blog",
  description: "A Next.js demo project",
};
```

> تستخدم لتعريف العنوان والـ SEO للصفحة.

---

## 5. Fetch Data in Next.js

### Client-side fetching:

```jsx
import { useEffect, useState } from "react";

export default function Post() {
  const [todo, setTodo] = useState({});

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/todos/1")
      .then((response) => response.json())
      .then((json) => setTodo(json));
  }, []);

  return (
    <div>
      <h1>{todo.title}</h1>
    </div>
  );
}
```

### Server-side fetching (Next.js 13+):

```jsx
export default async function Post() {
  const response = await fetch("https://jsonplaceholder.typicode.com/todos/1");
  const todo = await response.json();

  return (
    <div>
      <h1>{todo.title}</h1>
    </div>
  );
}
```

> `async/await` هنا أنظف وأسهل للصيانة، وكمان أسرع لأن البيانات تجيب قبل ما الصفحة تتعرض.

---

## 6. Handling Loading States

```jsx
// app/posts/loading.js
export default function Loading() {
  return <p>Loading posts...</p>;
}
```

```jsx
import { Suspense } from "react";
import PostsList from "./PostsList";

export default function PostsPage() {
  return (
    <Suspense fallback={<p>Loading...</p>}>
      <PostsList />
    </Suspense>
  );
}
```

---

## 7. نصائح إضافية:

* استخدم **Folders** لكل نوع من الصفحات أو الـ Components لتنظيم المشروع.
* حاول دائمًا استخدام **Server Components** لو البيانات مش متغيرة كثير لتقليل التحميل على العميل.
* للـ API calls الكبيرة استخدم **async/await** بدل `useEffect` للحصول على أفضل أداء.
* Link components في Next.js replace `<a>` HTML لتعمل routing بدون refresh.


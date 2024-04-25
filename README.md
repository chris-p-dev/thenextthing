Mixture of my understanding, [nextjs learn](https://nextjs.org/learn) and [docs](https://nextjs.org/docs)

### Project Structure

**/app** - App Router
**/pages** - Pages Router
**/public** - Static assets to be served
**/src** - Optional application source folder
**/next.config.js** - configuration file

### File Conventions

special files that have specific behavior
`.js`, `.jsx` or `.tsx` file extensions can be use for special files

**layout** - Shared UI for a segment and its children, automatically receives props for children, the children is a nested page.tsx
**page** - Unique UI of a route and make routes publicly accessible,autmatically receives props for children, the children is a nested page.tsx
**loading** - Loading UI for a segm and its children
**not-found** - Not found UI for a segment and its children
**error** - Error UI for a segment and its children

###### Colocation

we can combine other files (tests, assets, stylings ) inside `/app` director because `page` and `route` are made public(Routable)

### Routing

###### Nested Routing

![alt text](image-1.png)

file-system routing where folders are used to create nested routes. Each folder represents a route segment that maps to a URL segment.

You can create separate UIs for each route using `layout.tsx` and `page.tsx` files.

###### Navigating

`Link` component to redirect to pages

```ts
// some page or ui component
import Link from 'next/link';
<Link href="/dashboard/invoices" className=" text-whitetransition-colors hover:bg-blue-400" >
    Go Back
</Link>
```

### Server Actions

asynchronous functions that are executed on the server. They can be used in Server and Client Components to handle form submissions and data mutations in Next.js applications.

- eliminate the need to create API endpoints to mutate your data. Instead, you write asynchronous functions that execute on the server and can be invoked from your Client or Server Components.

###### Convention

use the inline function level or module level `"use server"`

Sample Action

### Concepts

### Partial Prerendering

serves a static page with dynamic holes for fast page initial load

- A static route shell is served, ensuring a fast initial load.
- The shell leaves holes where dynamic content will load in asynchronous.
- The async holes are streamed in parallel, reducing the overall load time of the page.

To enable this:

```ts #16
// next.config.js
    experimental: {
         ppr: true,
    },
```

Make use of Suspense `<Suspense>`

```ts
// some component with dynamic content
 return (
    <div className="w-full">
      <div className="flex w-full items-center justify-between">
        <h1 className={`${lusitana.className} text-2xl`}>Invoices</h1>
      </div>
      <div className="mt-4 flex items-center justify-between gap-2 md:mt-8">
        <Search placeholder="Search invoices..." />
        <CreateInvoice />
      </div>
      <Suspense key={query + currentPage} fallback={<InvoicesTableSkeleton />}>
        <Table query={query} currentPage={currentPage} />
      </Suspense>
      <div className="mt-5 flex w-full justify-center">
        <Pagination totalPages={totalPages} />
      </div>
    </div>
  );
}
```

### Error Handling

Adding these on the route segments can show or display page for Error Handling

###### Error Page

Added Error page on either root or specific route segment/file structure. Could be
.
├── ...
├── invoices
│ ├── [id]
│ ├── edit
│ ├── not-found.tsx
│ ├── error.tsx
│ ├── page.tsx
│ ├── layout.tsx
│
└── ...

```js
// somewhere server action or any API handling
....
export async function deleteInvoice(id: string) {
  throw new Error('Failed to Delete Invoice');
....

//  Error page will be shown through `error.tsx` page

```

###### Not Found

Added `not-found.tsx` file on a Route Segment

```ts
// some page.tsx
import { notFound } from 'next/navigation';
export default async function Page({ params }: { params: { id: string } }) {
  const id = params.id;
  if (!id) {
    notFound();
  }
...
// this will display `not-found.tsx`
```

### Accessibility

Improve Accessibility by using:

- **Semantic HTML**: Using semantic elements (`<input>`, `<option>`, etc) instead of `<div>`. This allows assistive technologies (AT) to focus on the input elements and provide appropriate contextual information to the user, making the form easier to navigate and understand.
- **Labelling**: Including <label> and the htmlFor attribute ensures that each form field has a descriptive text label. This improves AT support by providing context and also enhances usability by allowing users to click on the label to focus on the corresponding input field.
- **Focus Outline**: The fields are properly styled to show an outline when they are in focus. This is critical for accessibility as it visually indicates the active element on the page, helping both keyboard and screen reader users to understand where they are on the form. You can verify this by pressing tab.

### MetaData

- Filebase:
  - favicon.ico, apple-ico.jpg, icon.jpg - favicons and icons
  - opengraph-image.jpg, twitter-image.jpg - social media images
  - robots.txt - search engine crawling
  - sitemap.xml - websites structure

Adding metadata through `page` or `layout`

###### Individual Page

```ts
import { Metadata } from 'next';

export const metadata: Metadata = {
  title: 'Invoices | Acme Dashboard',
};
```

###### Template

```ts
// main layout
import { Metadata } from 'next';

export const metadata: Metadata = {
  title: {
    template: '%s | Acme Dashboard',
    default: 'Acme Dashboard',
  },
  description: 'The official Next.js Learn Dashboard built with App Router.',
  metadataBase: new URL('https://next-learn-dashboard.vercel.sh'),
};

// testpage
export const metadata: Metadata = {
  title: 'Invoices',
};

// this concatinates string into `Invoices | Acme Dashboard` when accessing the /testpage
```

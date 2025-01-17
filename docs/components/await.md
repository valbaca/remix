---
title: Await
toc: false
---

# `<Await>`

To get started with streaming data, check out the [Streaming Guide][streaming-guide-2].

The `<Await>` component is responsible for resolving deferred loader promises accessed from [`useLoaderData`][useloaderdata].

```tsx
import { Await } from "@remix-run/react";

<Suspense fallback={<div>Loading...</div>}>
  <Await resolve={somePromise}>
    {(resolvedValue) => <p>{resolvedValue}</p>}
  </Await>
</Suspense>;
```

## Props

### `resolve`

The resolve prop takes a promise from [`useLoaderData`][useloaderdata] to resolve when the data has streamed in.

```tsx
<Await resolve={somePromise} />
```

When the promise is not resolve, a parent suspense boundary's fallback will be rendered.

```tsx
<Suspense fallback={<div>Loading...</div>}>
  <Await resolve={somePromise} />
</Suspense>
```

When the promise is resolved, the children will be rendered.

### `children`

The `children` can be a render callback or a React element.

```tsx
<Await resolve={somePromise}>
  {(resolvedValue) => <p>{resolvedValue}</p>}
</Await>
```

If the `children` props is a React element, the resolved value will be accessible through `useAsyncValue` in the subtree.

```tsx
<Await resolve={somePromise}>
  <SomeChild />
</Await>
```

```tsx
import { useAsyncValue } from "@remix-run/react";

function SomeChild() {
  const value = useAsyncValue();
  return <p>{value}</p>;
}
```

### `errorElement`

The `errorElement` prop can be used to render an error boundary when the promise rejects.

```tsx
<Await errorElement={<div>Oops!</div>} />
```

The error can be accessed in the sub tree with [`useAsyncError`][use-async-error]

```tsx
<Await errorElement={<SomeChild />} />
```

```tsx
import { useAsyncError } from "@remix-run/react";

function SomeChild() {
  const error = useAsyncError();
  return <p>{error.message}</p>;
}
```

[defer]: ../utils/defer
[streaming-guide]: ../guides/streaming
[useloaderdata]: ../hooks/use-loader-data
[streaming-guide-2]: ../guides/streaming
[use-async-error]: ../hooks/use-async-error

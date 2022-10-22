---
theme: bricks
layout: intro
highlighter: prism
lineNumbers: true
colorSchema: auto
---

# Remix

Remix is a full stack web framework that lets you focus on the user interface and work back through web standards to deliver a fast, slick, and resilient user experience. People are gonna love using your stuff.

<div class="abs-br m-6 flex gap-2">
  <a href="https://remix.run/blog/seed-funding-for-remix" target="_blank" alt="Blog" class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-blog />
  </a>
  <a href="https://github.com/remix-run/remix" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
1. tell the history, click the blog icon

2. It's built on the Web Fetch API instead of Node.js. This enables Remix to run in any Node.js server like Vercel, Netlify, Architect, etc. as well as non-Node.js environments like Cloudflare Workers and Deno Deploy.
https://remix.run/docs/en/v1/pages/technical-explanation
-->

---
layout: items
---

# Create Remix Project

```shell
npx create-remix@latest
```

<div class="abs-bl m-6 flex gap-2">
  <a href="https://remix.run/docs/en/v1/pages/stacks" target="_blank" alt="Remix Stacks"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-application-web class="text-3xl animate-ping" />
  </a>
</div>

<!--
1. Go through with the Remix CLI
2. Explain about remix stacks, click the web app logo
3. we are using blues stack for this presentation
-->

---

# Data Loading

```ts {1-5|7-27|all} {maxHeight:'100'}
export async function loader() {
  return json({
    dictionaries: await getDictionaryList(),
  });
};

export default function DictionaryIndexPage() {
  const { dictionaries } = useLoaderData<typeof loader>();
  return (
    <div className="flex">
      <div className="h-screen bg-sky-200 p-5">
        <div className="mr-5">
          {
            dictionaries.map(dictionary => 
              <NavLink key={dictionary.word} to={dictionary.id}>
                <div className="pb-4 text-blue-400 hover:text-blue-700">{dictionary.word}</div>
              </NavLink>
            )
          }
        </div>
      </div>
      <div className="flex-1">
        <Outlet />
      </div>
    </div>
  );
}
```

---

# Dynamic Params

```ts
export async function loader({params}: LoaderArgs) {
  invariant(params.dictionaryId, "Dictionary not found");
  return json({
    dictionary: await getDictionaryById({id: params.dictionaryId}),
  });
};

export default function DictionaryDetailPage () {
  const { dictionary } = useLoaderData<typeof loader>();
  const { dictionaryId } = useParams();
  return (
    <div>
      {dictionary?.word}
      {dictionary?.description}
    </div>
  );
}
```

---

# Routing

<div class="m-6 flex gap-2">
  <a href="https://remix-routing-demo.netlify.app/" target="_blank" alt="Remix Routing"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-application-web class="text-5xl animate-ping" />
  </a>
</div>


---

# Mutations

```ts {1-5|1-5,8|1-5,8,19|1-5|1-5,9-10|1-5,9-10,20-25}
export async function action({ request }) {
  const body = await request.formData();
  const name = body.get("visitorsName");
  return json({ message: `Hello, ${name}` });
}

export default function Invoices() {
  const data = useActionData();
  const transition = useTransition();
  const isCreating = transition.submission?.formData.get("intent") === "create";
  return (
    <Form method="post">
      <p>
        <label>
          What is your name?
          <input type="text" name="visitorsName" />
        </label>
      </p>
      <p>{data ? data.message : "Waiting..."}</p>
      <button
        type="submit"
        name="intent"
        value="create"
        disabled={isCreating}
      >
    </Form>
  );
}
```

---

# Prefetching

```ts
<>
  <Link /> {/* defaults to "none" */}
  <Link prefetch="none" />
  <Link prefetch="intent" />
  <Link prefetch="render" />
</>
```

---

# Errors

```ts {1-10|12-25|all}
export function ErrorBoundary({ error }: { error: Error }) {
  return (
    <div className="p-5">
      <h1>Error</h1>
      <p>{error.message}</p>
      <p>The stack trace is:</p>
      <pre>{error.stack}</pre>
    </div>
  );
}

export function CatchBoundary() {
  const caught = useCatch();
  if (caught.status === 404) {
    return (
      <div className="p-5">
        <h1>Caught</h1>
        <p>Status: {caught.status}</p>
        <p>Message: {caught.data}</p>
      </div>
    );
  }
  throw new Error("Error is coming from Catch Boundary")
}
```

---
layout: center
class: text-center
---

# Thank You

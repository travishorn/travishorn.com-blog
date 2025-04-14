---
title: "Cursor Rules for SvelteKit Development"
datePublished: Sat Apr 12 2025 11:00:44 GMT+0000 (Coordinated Universal Time)
cuid: cm9e3um1m000t09le4o07cau1
slug: cursor-rules-for-sveltekit-development
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1744379590510/f2cbf036-a0a8-4cc8-9069-c7a6ae7ff7cb.png
tags: svelte, cursor, tailwind-css, sveltekit

---

Cursor, the AI-first code editor, significantly speeds up my development workflow, especially when working with modern web frameworks. However, like many AI models, its training data lags slightly behind the latest releases of frameworks like Svelte, SvelteKit, and Tailwind CSS. This can sometimes lead to the AI suggesting code based on older patterns, potentially causing compatibility issues or preventing me from leveraging the newest features.

Thankfully, Cursor provides a powerful "Rules" feature that can be used to address this scenario. By defining specific rules within your project, you can guide Cursor's AI agents, instructing them to adhere to the standards, syntax, and best practices of the exact versions you're using.

In this post, I'll walk you through how I set up custom Cursor rules in my SvelteKit projects to ensure better compatibility and code generation for Svelte 5, SvelteKit 2, and Tailwind CSS 4 (current versions as of this writing). This simple setup helps keep the AI suggestions relevant.

The process involves creating a `.cursor/rules` directory in your project root and adding three specific Markdown Configuration (.mdc) files.

First, I create `.cursor/rules/svelte-5.mdc`. Under **Rule Type**, I choose **Auto Attached**. Under **File pattern matches**, I enter `*.svelte` and `*.ts`. Here’s the content of that file.

````markdown
# Svelte 5

This project uses the newer Svelte 5 instead of the more common Svelte 4.

Version 5 comes with an overhauled syntax and reactivity system. While it may look different at first, you'll soon notice many similarities. This guide goes over the changes in detail and shows you how to upgrade. Along with it, we also provide information on _why_ we did these changes.

## Reactivity syntax changes

At the heart of Svelte 5 is the new runes API. Runes are basically compiler instructions that inform Svelte about reactivity. Syntactically, runes are functions starting with a dollar-sign.

### let → $state

In Svelte 4, a `let` declaration at the top level of a component was implicitly reactive. In Svelte 5, things are more explicit: a variable is reactive when created using the `$state` rune. Let's migrate the counter to runes mode by wrapping the counter in `$state`.

Old, Svelte 4 syntax:

```svelte
<script>
	let count = 0;
</script>
```

New, Svelte 5 syntax:

```svelte
<script>
	let count = $state(0);
</script>
```

Nothing else changes. `count` is still the number itself, and you read and write directly to it, without a wrapper like `.value` or `getCount()`.

### $: → $derived/$effect

In Svelte 4, a `$:` statement at the top level of a component could be used to declare a derivation, i.e. state that is entirely defined through a computation of other state. In Svelte 5, this is achieved using the `$derived` rune.

Old, Svelte 4 syntax:

```svelte
<script>
	let count = 0;
	$: double = count * 2;
</script>
```

New, Svelte 5 syntax:

```svelte
<script>
	let count = $state(0);
	const double = $derived(count * 2);
</script>
```

As with `$state`, nothing else changes. `double` is still the number itself, and you read it directly, without a wrapper like `.value` or `getDouble()`.

Sometimes you need to create complex derivations that don't fit inside a short expression. In these cases, you can use `$derived.by` which accepts a function as its argument.

```svelte
<script>
	let numbers = $state([1, 2, 3]);
	let total = $derived.by(() => {
		let total = 0;
		for (const n of numbers) {
			total += n;
		}
		return total;
	});
</script>

<button onclick={() => numbers.push(numbers.length + 1)}>
	{numbers.join(' + ')} = {total}
</button>
```

In Svelte 4, a `$:` statement could be used to create side effects. In Svelte 5, this is achieved using the `$effect` rune.

Old, Svelte 4 syntax:

```svelte
<script>
	let count = $state(0);

	$: {
		if (count > 5) {
			alert('Count is too high!');
		}
	}
</script>
```

New, Svelte 5 syntax:

```svelte
<script>
	let count = 0;

	$effect(() => {
		if (count > 5) {
			alert('Count is too high!');
		}
	});
</script>
```

### export let → $props

In Svelte 4, properties of a component were declared using `export let`. Each property was one declaration. In Svelte 5, all properties are declared through the `$props` rune, through destructuring.

Old, Svelte 4 syntax:

```svelte
<script>
	export let optional = 'unset';
	export let required;
</script>
```

New, Svelte 5 syntax:

```svelte
<script>
	let { optional = 'unset', required } = $props();
</script>
```

There are multiple cases where declaring properties becomes less straightforward than having a few `export let` declarations:

- you want to rename the property, for example because the name is a reserved identifier (e.g. `class`)
- you don't know which other properties to expect in advance
- you want to forward every property to another component

All these cases need special syntax in Svelte 4:

- renaming: `export { klass as class}`
- other properties: `$$restProps`
- all properties `$$props`

In Svelte 5, the `$props` rune makes this straightforward without any additional Svelte-specific syntax:

- renaming: use property renaming `let { class: klass } = $props();`
- other properties: use spreading `let { foo, bar, ...rest } = $props();`
- all properties: don't destructure `let props = $props();`

Old, Svelte 4 syntax:

```svelte
<script>
	let klass = '';
	export { klass as class };
</script>

<button class={klass} {...$$restProps}>click me</button>
```

New, Svelte 5 syntax:

```svelte
<script>
	let { class: klass, ...rest } = $props();
</script>

<button class={klass} {...rest}>click me</button>
```

## Event changes

Event handlers have been given a facelift in Svelte 5. Whereas in Svelte 4 we use the `on:` directive to attach an event listener to an element, in Svelte 5 they are properties like any other (in other words - remove the colon).

Old, Svelte 4 syntax:

```svelte
<script>
	let count = 0;
</script>

<button on:click={() => count++}>
	clicks: {count}
</button>
```

New, Svelte 5 syntax:

```svelte
<script>
	let count = $state(0);
</script>

<button onclick={() => count++}>
	clicks: {count}
</button>
```

Since they're just properties, you can use the normal shorthand syntax...

```svelte
<script>
	let count = $state(0);

	function onclick() {
		count++;
	}
</script>

<button {onclick}>
	clicks: {count}
</button>
```

...though when using a named event handler function it's usually better to use a more descriptive name.

### Component events

In Svelte 4, components could emit events by creating a dispatcher with `createEventDispatcher`.

This function is deprecated in Svelte 5. Instead, components should accept _callback props_ - which means you then pass functions as properties to these components.

Old, Svelte 4 syntax:

```svelte
<!--- file: App.svelte --->
<script>
	import Pump from './Pump.svelte';

	let size = 15;
	let burst = false;

	function reset() {
		size = 15;
		burst = false;
	}
</script>

<Pump
	on:inflate={(power) => {
		size += power.detail;
		if (size > 75) burst = true;
	}}
	on:deflate={(power) => {
		if (size > 0) size -= power.detail;
	}}
/>

{#if burst}
	<button onclick={reset}>new balloon</button>
	<span class="boom">💥</span>
{:else}
	<span class="balloon" style="scale: {0.01 * size}"> 🎈 </span>
{/if}
```

```svelte
<!--- file: Pump.svelte --->
<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	let power = 0;
</script>

<button onclick={() => dispatch('inflate', power)}> inflate </button>
<button onclick={() => dispatch('deflate', power)}> deflate </button>
<button onclick={() => power--}>-</button>
Pump power: {power}
<button onclick={() => power++}>+</button>
```

New, Svelte 5 syntax:

```svelte
<!--- file: App.svelte --->
<script>
	import Pump from './Pump.svelte';

	let size = $state(15);
	let burst = $state(false);

	function reset() {
		size = 15;
		burst = false;
	}
</script>

<Pump
	inflate={(power) => {
		size += power;
		if (size > 75) burst = true;
	}}
	deflate={(power) => {
		if (size > 0) size -= power;
	}}
/>

{#if burst}
	<button onclick={reset}>new balloon</button>
	<span class="boom">💥</span>
{:else}
	<span class="balloon" style="scale: {0.01 * size}"> 🎈 </span>
{/if}
```

```svelte
<!--- file: Pump.svelte --->
<script>
	let { inflate, deflate } = $props();
	let power = $state(5);
</script>

<button onclick={() => inflate(power)}> inflate </button>
<button onclick={() => deflate(power)}> deflate </button>
<button onclick={() => power--}>-</button>
Pump power: {power}
<button onclick={() => power++}>+</button>
```

### Bubbling events

Instead of doing `<button on:click>` to 'forward' the event from the element to the component, the component should accept an `onclick` callback prop.

Old, Svelte 4 syntax:

```svelte
<button on:click> click me </button>
```

New, Svelte 5 syntax:

```svelte
<script>
	let { onclick } = $props();
</script>

<button {onclick}> click me </button>
```

Note that this also means you can 'spread' event handlers onto the element along with other props instead of tediously forwarding each event separately.

Old, Svelte 4 syntax:

```svelte
<button {...$$props} on:click on:keydown on:all_the_other_stuff> click me </button>
```

New, Svelte 5 syntax:

```svelte
<script>
	let props = $props();
</script>

<button {...props}> click me </button>
```

### Event modifiers

In Svelte 4, you could add event modifiers to handlers:

```svelte
<button on:click|once|preventDefault={handler}>...</button>
```

Modifiers are specific to `on:` and as such do not work with modern event handlers. Adding things like `event.preventDefault()` inside the handler itself is preferable, since all the logic lives in one place rather than being split between handler and modifiers.

Since event handlers are just functions, you can create your own wrappers as necessary:

```svelte
<script>
	function once(fn) {
		return function (event) {
			if (fn) fn.call(this, event);
			fn = null;
		};
	}

	function preventDefault(fn) {
		return function (event) {
			event.preventDefault();
			fn.call(this, event);
		};
	}
</script>

<button onclick={once(preventDefault(handler))}>...</button>
```

There are three modifiers — `capture`, `passive` and `nonpassive` — that can't be expressed as wrapper functions, since they need to be applied when the event handler is bound rather than when it runs.

For `capture`, we add the modifier to the event name:

```svelte
<button onclickcapture={...}>...</button>
```

Changing the `passive` option of an event handler, meanwhile, is not something to be done lightly. If you have a use case for it — and you probably don't! — then you will need to use an action to apply the event handler yourself.

### Multiple event handlers

In Svelte 4, this was possible:

```svelte
<button on:click={one} on:click={two}>...</button>
```

Duplicate attributes/properties on elements — which now includes event handlers — are not allowed. Instead, do this:

```svelte
<button
	onclick={(e) => {
		one(e);
		two(e);
	}}
>
	...
</button>
```

When spreading props, local event handlers must go _after_ the spread, or they risk being overwritten:

```svelte
<button
	{...props}
	onclick={(e) => {
		doStuff(e);
		props.onclick?.(e);
	}}
>
	...
</button>
```

## Snippets instead of slots

In Svelte 4, content could be passed to components using slots. Svelte 5 replaces them with snippets which are more powerful and flexible, and as such slots are deprecated in Svelte 5.

### Default content

In Svelte 4, the easiest way to pass a piece of UI to the child was using a `<slot />`. In Svelte 5, this is done using the `children` prop instead, which is then shown with `{@render children()}`:

Old, Svelte 4 syntax:

```svelte
<slot />
```

New, Svelte 5 syntax:

```svelte
<script>
	let { children } = $props();
</script>

{@render children?.()}
```

### Multiple content placeholders

If you wanted multiple UI placeholders, you had to use named slots. In Svelte 5, use props instead, name them however you like and `{@render ...}` them:

Old, Svelte 4 syntax:

```svelte
<header>
	<slot name="header" />
</header>

<main>
	<slot name="main" />
</main>

<footer>
	<slot name="footer" />
</footer>
```

New, Svelte 5 syntax:

```svelte
<script>
	let { header, main, footer } = $props();
</script>

<header>
	{@render header()}
</header>

<main>
	{@render main()}
</main>

<footer>
	{@render footer()}
</footer>
```

### Passing data back up

In Svelte 4, you would pass data to a `<slot />` and then retrieve it with `let:` in the parent component. In Svelte 5, snippets take on that responsibility.

Old, Svelte 4 syntax:

```svelte
<!--- file: App.svelte --->
<script>
	import List from './List.svelte';
</script>

<List items={['one', 'two', 'three']} let:item>
	<span>{text}</span>
	<span slot="empty">No items yet</span>
</List>
```

```svelte
<!--- file: List.svelte --->
<script>
	export let items;
</script>

{#if items.length}
	<ul>
		{#each items as entry}
			<li>
				<slot item={entry} />
			</li>
		{/each}
	</ul>
{:else}
	<slot name="empty" />
{/if}
```

New, Svelte 5 syntax:

```svelte
<!--- file: App.svelte --->
<script>
	import List from './List.svelte';
</script>

<List items={['one', 'two', 'three']}>
	{#snippet item(text)}
		<span>{text}</span>
	{/snippet}

	{#snippet empty()}
		<span>No items yet</span>
	{/snippet}
</List>
```

```svelte
<!--- file: List.svelte --->
<script>
	let { items, item, empty } = $props();
</script>

{#if items.length}
	<ul>
		{#each items as entry}
			<li>
				{@render item(entry)}
			</li>
		{/each}
	</ul>
{:else}
	{@render empty?.()}
{/if}
```
````

Then, I create `.cursor/rules/sveltekit-2.mdc`. Under **Rule Type**, I choose **Auto Attached**. Under **File pattern matches**, I enter `*.svelte` and `*.ts`. Here’s the content of that file.

````markdown
# SvelteKit 2

This project uses the newer SvelteKit 2 instead of the older, but more common SvelteKit 1.

Using SvelteKit version 2 instead of version 1 is mostly seamless. There are a few breaking changes to note, which are listed here.

## `redirect` and `error` are no longer thrown by you

Previously, you had to `throw` the values returned from `error(...)` and `redirect(...)` yourself. In SvelteKit 2 this is no longer the case — calling the functions is sufficient.

Old, SvelteKit 1 syntax:

```js
import { error } from '@sveltejs/kit'

// ...
throw error(500, 'something went wrong');
```

New, SvelteKit 2 syntax:

```js
import { error } from '@sveltejs/kit'

// ...
error(500, 'something went wrong');
```

Do not use `error` or `redirect` inside a `try {...}` block.

## path is required when setting cookies

When receiving a `Set-Cookie` header that doesn't specify a `path`, browsers will set the cookie path to the parent of the resource in question. This behaviour isn't particularly helpful or intuitive, and frequently results in bugs because the developer expected the cookie to apply to the domain as a whole.

As of SvelteKit 2.0, you need to set a `path` when calling `cookies.set(...)`, `cookies.delete(...)` or `cookies.serialize(...)` so that there's no ambiguity. Most of the time, you probably want to use `path: '/'`, but you can set it to whatever you like, including relative paths — `''` means 'the current path', `'.'` means 'the current directory'.

Old, SvelteKit 1 syntax:

```js
/** @type {import('./$types').PageServerLoad} */
export function load({ cookies }) {
	cookies.set(name, value);
	return { response }
}
```

New, SvelteKit 2 syntax:

```js
/** @type {import('./$types').PageServerLoad} */
export function load({ cookies }) {
	cookies.set(name, value, { path: '/' });
	return { response }
}
```

## Top-level promises are no longer awaited

In SvelteKit version 1, if the top-level properties of the object returned from a `load` function were promises, they were automatically awaited. With the introduction of streaming this behavior became a bit awkward as it forces you to nest your streamed data one level deep.

As of version 2, SvelteKit no longer differentiates between top-level and non-top-level promises. To get back the blocking behavior, use `await` (with `Promise.all` to prevent waterfalls, where appropriate).

Old, SvelteKit 1 syntax:

```js
// If you have a single promise
/** @type {import('./$types').PageServerLoad} */
export function load({ fetch }) {
	const response = fetch(url).then(r => r.json());
	return { response }
}
```

```js
// If you have multiple promises
/** @type {import('./$types').PageServerLoad} */
export  function load({ fetch }) {
    const a = fetch(url1).then(r => r.json());
    const b = fetch(url2).then(r => r.json());

	return { a, b };
}
```

New, SvelteKit 2 syntax:

```js
// If you have a single promise
/** @type {import('./$types').PageServerLoad} */
export async function load({ fetch }) {
	const response = await fetch(url).then(r => r.json());
	return { response }
}
```

```js
// If you have multiple promises
/** @type {import('./$types').PageServerLoad} */
export  function load({ fetch }) {
    const [a, b] = await Promise.all([
	  fetch(url1).then(r => r.json()),
	  fetch(url2).then(r => r.json()),
	]);

	return { a, b };
}
```

## goto(...) changes

`goto(...)` no longer accepts external URLs. To navigate to an external URL, use `window.location.href = url`. The `state` object now determines `$page.state` and must adhere to the `App.PageState` interface, if declared.

## paths are now relative by default

In SvelteKit 1, `%sveltekit.assets%` in your `app.html` was replaced with a relative path by default (i.e. `.` or `..` or `../..` etc, depending on the path being rendered) during server-side rendering unless the `paths.relative` config option was explicitly set to `false`. The same was true for `base` and `assets` imported from `$app/paths`, but only if the `paths.relative` option was explicitly set to `true`.

This inconsistency is fixed in version 2. Paths are either always relative or always absolute, depending on the value of `paths.relative`. It defaults to `true` as this results in more portable apps: if the `base` is something other than the app expected (as is the case when viewed on the Internet Archive, for example) or unknown at build time (as is the case when deploying to IPFS and so on), fewer things are likely to break.

## Server fetches are not trackable anymore

Previously it was possible to track URLs from `fetch`es on the server in order to rerun load functions. This poses a possible security risk (private URLs leaking), and as such it was behind the `dangerZone.trackServerFetches` setting, which is now removed.

## `preloadCode` arguments must be prefixed with `base`

SvelteKit exposes two functions, `preloadCode` and `preloadData`, for programmatically loading the code and data associated with a particular path. In version 1, there was a subtle inconsistency — the path passed to `preloadCode` did not need to be prefixed with the `base` path (if set), while the path passed to `preloadData` did.

This is fixed in SvelteKit 2 — in both cases, the path should be prefixed with `base` if it is set.

Additionally, `preloadCode` now takes a single argument rather than _n_ arguments.

## `resolvePath` has been removed

SvelteKit 1 included a function called `resolvePath` which allows you to resolve a route ID (like `/blog/[slug]`) and a set of parameters (like `{ slug: 'hello' }`) to a pathname. Unfortunately the return value didn't include the `base` path, limiting its usefulness in cases where `base` was set.

As such, SvelteKit 2 replaces `resolvePath` with a (slightly better named) function called `resolveRoute`, which is imported from `$app/paths` and which takes `base` into account.

Old, SvelteKit 1 syntax:

```js
import { resolvePath } from '@sveltejs/kit';
import { base } from '$app/paths';

const path = base + resolvePath('/blog/[slug]', { slug });
```

New, SvelteKit 2 syntax:

```js
import { resolveRoute } from '$app/paths';

const path = resolveRoute('/blog/[slug]', { slug });
```

## Improved error handling

Errors are handled inconsistently in SvelteKit 1. Some errors trigger the `handleError` hook but there is no good way to discern their status (for example, the only way to tell a 404 from a 500 is by seeing if `event.route.id` is `null`), while others (such as 405 errors for `POST` requests to pages without actions) don't trigger `handleError` at all, but should. In the latter case, the resulting `$page.error` will deviate from the `App.Error` type, if it is specified.

SvelteKit 2 cleans this up by calling `handleError` hooks with two new properties: `status` and `message`. For errors thrown from your code (or library code called by your code) the status will be `500` and the message will be `Internal Error`. While `error.message` may contain sensitive information that should not be exposed to users, `message` is safe.

## Dynamic environment variables cannot be used during prerendering

The `$env/dynamic/public` and `$env/dynamic/private` modules provide access to _run time_ environment variables, as opposed to the _build time_ environment variables exposed by `$env/static/public` and `$env/static/private`.

During prerendering in SvelteKit 1, they are one and the same. As such, prerendered pages that make use of 'dynamic' environment variables are really 'baking in' build time values, which is incorrect. Worse, `$env/dynamic/public` is populated in the browser with these stale values if the user happens to land on a prerendered page before navigating to dynamically-rendered pages.

Because of this, dynamic environment variables can no longer be read during prerendering in SvelteKit 2 — you should use the `static` modules instead. If the user lands on a prerendered page, SvelteKit will request up-to-date values for `$env/dynamic/public` from the server (by default from a module called `/_app/env.js`) instead of reading them from the server-rendered HTML.

## `form` and `data` have been removed from `use:enhance` callbacks

If you provide a callback to `use:enhance`, it will be called with an object containing various useful properties.

In SvelteKit 1, those properties included `form` and `data`. These were deprecated some time ago in favour of `formElement` and `formData`, and have been removed altogether in SvelteKit 2.

## Forms containing file inputs must use `multipart/form-data`

If a form contains an `<input type="file">` but does not have an `enctype="multipart/form-data"` attribute, non-JS submissions will omit the file. SvelteKit 2 will throw an error if it encounters a form like this during a `use:enhance` submission to ensure that your forms work correctly when JavaScript is not present.

## Generated `tsconfig.json` is more strict

Previously, the generated `tsconfig.json` was trying its best to still produce a somewhat valid config when your `tsconfig.json` included `paths` or `baseUrl`. In SvelteKit 2, the validation is more strict and will warn when you use either `paths` or `baseUrl` in your `tsconfig.json`. These settings are used to generate path aliases and you should use the `alias` config option in your `svelte.config.js` instead, to also create a corresponding alias for the bundler.

## `getRequest` no longer throws errors

The `@sveltejs/kit/node` module exports helper functions for use in Node environments, including `getRequest` which turns a Node `ClientRequest` into a standard `Request` object.

In SvelteKit 1, `getRequest` could throw if the `Content-Length` header exceeded the specified size limit. In SvelteKit 2, the error will not be thrown until later, when the request body (if any) is being read. This enables better diagnostics and simpler code.

## `vitePreprocess` is no longer exported from `@sveltejs/kit/vite`

Since `@sveltejs/vite-plugin-svelte` is now a peer dependency, SvelteKit 2 no longer re-exports `vitePreprocess`. You should import it directly from `@sveltejs/vite-plugin-svelte`.

## Updated dependency requirements

SvelteKit 2 requires Node `18.13` or higher, and the following minimum dependency versions:

- `svelte@4`
- `vite@5`
- `typescript@5`
- `@sveltejs/vite-plugin-svelte@3` (this is now required as a `peerDependency` of SvelteKit — previously it was directly depended upon)
- `@sveltejs/adapter-cloudflare@3` (if you're using this adapter)
- `@sveltejs/adapter-cloudflare-workers@2` (if you're using this adapter)
- `@sveltejs/adapter-netlify@3` (if you're using this adapter)
- `@sveltejs/adapter-node@2` (if you're using this adapter)
- `@sveltejs/adapter-static@3` (if you're using this adapter)
- `@sveltejs/adapter-vercel@4` (if you're using this adapter)

As part of the TypeScript upgrade, the generated `tsconfig.json` (the one your `tsconfig.json` extends from) now uses `"moduleResolution": "bundler"` (which is recommended by the TypeScript team, as it properly resolves types from packages with an `exports` map in package.json) and `verbatimModuleSyntax` (which replaces the existing `importsNotUsedAsValues ` and `preserveValueImports` flags — if you have those in your `tsconfig.json`, remove them).

## SvelteKit 2.12: $app/stores deprecated

SvelteKit 2.12 introduced `$app/state` based on the Svelte 5 runes API. `$app/state` provides everything that `$app/stores` provides but with more flexibility as to where and how you use it. Most importantly, the `page` object is now fine-grained, e.g. updates to `page.state` will not invalidate `page.data` and vice-versa.

As a consequence, `$app/stores` is deprecated. Do not use `$app/stores`. Most of the replacements should be pretty simple: Replace the `$app/stores` import with `$app/state` and remove the `$` prefixes from the usage sites.

Old, SvelteKit 1 syntax:

```svelte
<script>
	import { page } from '$app/stores';
</script>

{$page.data}
```

New, SvelteKit 2 syntax:

```svelte
<script>
	import { page } from '$app/state';
</script>

{page.data}
```
````

Finally, I create `.cursor/rules/tailwind-css-4.mdc`. Under **Rule Type**, I choose **Auto Attached**. Under **File pattern matches**, I enter `*.svelte` and `*.css`. Here’s the content of that file.

```markdown
# Tailwind CSS 4

This project uses the newer Tailwind CSS 4 instead of the older, but more common Tailwind CSS 3.

## Core Changes

- **CSS-first configuration**: Configuration is now done in CSS instead of JavaScript
  - Use `@theme` directive in CSS instead of `tailwind.config.js`
  - Example:
    ```css
    @import "tailwindcss";

    @theme {
      --font-display: "Satoshi", "sans-serif";
      --breakpoint-3xl: 1920px;
      --color-avocado-500: oklch(0.84 0.18 117.33);
      --ease-fluid: cubic-bezier(0.3, 0, 0, 1);
    }
    ```
- Legacy `tailwind.config.js` files can still be imported using the `@config` directive:
  ```css
  @import "tailwindcss";
  @config "../../tailwind.config.js";
  ```
- **CSS import syntax**: Use `@import "tailwindcss"` instead of `@tailwind` directives
  - Old: `@tailwind base; @tailwind components; @tailwind utilities;`
  - New: `@import "tailwindcss";`

- **Package changes**:
  - PostCSS plugin is now `@tailwindcss/postcss` (not `tailwindcss`)
  - CLI is now `@tailwindcss/cli`
  - Vite plugin is `@tailwindcss/vite`
  - No need for `postcss-import` or `autoprefixer` anymore

- **Native CSS cascade layers**: Uses real CSS `@layer` instead of Tailwind's custom implementation

## Theme Configuration

- **CSS theme variables**: All design tokens are available as CSS variables
  - Namespace format: `--category-name` (e.g., `--color-blue-500`, `--font-sans`)
  - Access in CSS: `var(--color-blue-500)`
  - Available namespaces:
    - `--color-*` : Color utilities like `bg-red-500` and `text-sky-300`
    - `--font-*` : Font family utilities like `font-sans`
    - `--text-*` : Font size utilities like `text-xl`
    - `--font-weight-*` : Font weight utilities like `font-bold`
    - `--tracking-*` : Letter spacing utilities like `tracking-wide`
    - `--leading-*` : Line height utilities like `leading-tight`
    - `--breakpoint-*` : Responsive breakpoint variants like `sm:*`
    - `--container-*` : Container query variants like `@sm:*` and size utilities like `max-w-md`
    - `--spacing-*` : Spacing and sizing utilities like `px-4` and `max-h-16`
    - `--radius-*` : Border radius utilities like `rounded-sm`
    - `--shadow-*` : Box shadow utilities like `shadow-md`
    - `--inset-shadow-*` : Inset box shadow utilities like `inset-shadow-xs`
    - `--drop-shadow-*` : Drop shadow filter utilities like `drop-shadow-md`
    - `--blur-*` : Blur filter utilities like `blur-md`
    - `--perspective-*` : Perspective utilities like `perspective-near`
    - `--aspect-*` : Aspect ratio utilities like `aspect-video`
    - `--ease-*` : Transition timing function utilities like `ease-out`
    - `--animate-*` : Animation utilities like `animate-spin`
  

- **Simplified theme configuration**: Many utilities no longer need theme configuration
  - Utilities like `grid-cols-12`, `z-40`, and `opacity-70` work without configuration
  - Data attributes like `data-selected:opacity-100` don't need configuration

- **Dynamic spacing scale**: Derived from a single spacing value
  - Default: `--spacing: 0.25rem`
  - Every multiple of the base value is available (e.g., `mt-21` works automatically)

- **Overriding theme namespaces**:
  - Override entire namespace: `--font-*: initial;`
  - Override entire theme: `--*: initial;`


## New Features

- **Container query support**: Built-in now, no plugin needed
  - `@container` for container context
  - `@sm:`, `@md:`, etc. for container-based breakpoints
  - `@max-md:` for max-width container queries
  - Combine with `@min-md:@max-xl:hidden` for ranges

- **3D transforms**:
  - `transform-3d` enables 3D transforms
  - `rotate-x-*`, `rotate-y-*`, `rotate-z-*` for 3D rotation
  - `scale-z-*` for z-axis scaling
  - `translate-z-*` for z-axis translation
  - `perspective-*` utilities (`perspective-near`, `perspective-distant`, etc.)
  - `perspective-origin-*` utilities
  - `backface-visible` and `backface-hidden`

- **Gradient enhancements**:
  - Linear gradient angles: `bg-linear-45` (renamed from `bg-gradient-*`)
  - Gradient interpolation: `bg-linear-to-r/oklch`, `bg-linear-to-r/srgb`
  - Conic and radial gradients: `bg-conic`, `bg-radial-[at_25%_25%]`

- **Shadow enhancements**:
  - `inset-shadow-*` and `inset-ring-*` utilities
  - Can be composed with regular `shadow-*` and `ring-*`

- **New CSS property utilities**:
  - `field-sizing-content` for auto-resizing textareas
  - `scheme-light`, `scheme-dark` for `color-scheme` property
  - `font-stretch-*` utilities for variable fonts

## New Variants

- **Composable variants**: Chain variants together
  - Example: `group-has-data-potato:opacity-100`

- **New variants**:
  - `starting` variant for `@starting-style` transitions
  - `not-*` variant for `:not()` pseudo-class
  - `inert` variant for `inert` attribute
  - `nth-*` variants (`nth-3:`, `nth-last-5:`, `nth-of-type-4:`, `nth-last-of-type-6:`)
  - `in-*` variant (like `group-*` but without adding `group` class)
  - `open` variant now supports `:popover-open`
  - `**` variant for targeting all descendants

## Custom Extensions

- **Custom utilities**: Use `@utility` directive
  ```css
  @utility tab-4 {
    tab-size: 4;
  }
  ```

- **Custom variants**: Use `@variant` directive
  ```css
  @variant pointer-coarse (@media (pointer: coarse));
  @variant theme-midnight (&:where([data-theme="midnight"] *));
  ```

- **Plugins**: Use `@plugin` directive
  ```css
  @plugin "@tailwindcss/typography";
  ```

## Breaking Changes

- **Removed deprecated utilities**:
  - `bg-opacity-*` → Use `bg-black/50` instead
  - `text-opacity-*` → Use `text-black/50` instead
  - And others: `border-opacity-*`, `divide-opacity-*`, etc.

- **Renamed utilities**:
  - `shadow-sm` → `shadow-xs` (and `shadow` → `shadow-sm`)
  - `drop-shadow-sm` → `drop-shadow-xs` (and `drop-shadow` → `drop-shadow-sm`)
  - `blur-sm` → `blur-xs` (and `blur` → `blur-sm`)
  - `rounded-sm` → `rounded-xs` (and `rounded` → `rounded-sm`)
  - `outline-none` → `outline-hidden` (for the old behavior)

- **Default style changes**:
  - Default border color is now `currentColor` (was `gray-200`)
  - Default `ring` width is now 1px (was 3px)
  - Placeholder text now uses current color at 50% opacity (was `gray-400`)
  - Hover styles only apply on devices that support hover (`@media (hover: hover)`)

- **Syntax changes**:
  - CSS variables in arbitrary values: `bg-(--brand-color)` instead of `bg-[--brand-color]`
  - Stacked variants now apply left-to-right (not right-to-left)
  - Use CSS variables instead of `theme()` function 

## Advanced Configuration

- **Using a prefix**:
  ```css
  @import "tailwindcss" prefix(tw);
  ```
  - Results in classes like `tw:flex`, `tw:bg-red-500`, `tw:hover:bg-red-600`

- **Source detection**:
  - Automatic by default (ignores `.gitignore` files and binary files)
  - Add sources: `@source "../node_modules/@my-company/ui-lib";`
  - Disable automatic detection: `@import "tailwindcss" source(none);`

- **Legacy config files**:
  ```css
  @import "tailwindcss";
  @config "../../tailwind.config.js";
  ```

- **Dark mode configuration**:
  ```css
  @import "tailwindcss";
  @variant dark (&:where(.dark, .dark *));
  ```

- **Container customization**: Extend with `@utility`
  ```css
  @utility container {
    margin-inline: auto;
    padding-inline: 2rem;
  }
  ```

- **Using `@apply` in Vue/Svelte**:
  ```html
  <style>
    @import "../../my-theme.css" theme(reference);
    /* or */
    @import "tailwindcss/theme" theme(reference);
    
    h1 {
      @apply font-bold text-2xl text-red-500;
    }
  </style>
  ```
```

With these three rule files placed within your `.cursor/rules` directory in your project root, you've instructed Cursor's AI agents to prioritize the syntax, APIs, and conventions of the latest versions you're working with.

This significantly improves the quality and relevance of AI-generated code suggestions and edits. It reduces the friction of having to manually correct outdated patterns and helps ensure your project leverages the modern best practices defined by Svelte 5, SvelteKit 2, and Tailwind CSS 4.

Don't be afraid to adapt these examples to match your coding style or project requirements. You can modify the file patterns or the instructional content within the .mdc files to suit other libraries, different versions, or specific coding standards used by your team. Leveraging Cursor's rules is a fantastic way to fine-tune the AI to your precise development context, making an already powerful tool even more effective.
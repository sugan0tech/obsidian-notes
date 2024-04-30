Nextjs of svelte

Handles:
- routing
- server side rendering
- type script integration


### Routing
- `+page.svelte` for intro page
- `+layout.svelte` for the layouts ( can be headers and footers )
```jsx
<nav>
  <a href="/"> home </a>
  <a href="/about"> about </a>
</nav>

<slot /> // will add the +page.svelte contents here
```
- Routing structure
	- follows the directory structure
		- `routes`
			- `+page.svelte`
			- `+layout.svelte` will be applied to all the following routes
			- `/about`
				- `+page.svelte`
			- `/login`
				- `+page.svelte`


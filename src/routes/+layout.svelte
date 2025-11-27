<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';
	import Blanchor from '$lib/Blanchor.svelte';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children()}

<footer>
  <p>Built using <Blanchor href="https://kit.svelte.dev/">SvelteKit</Blanchor> and <Blanchor href="https://www.typescriptlang.org/">TypeScript</Blanchor></p>
  <p>Github Repo: <Blanchor href="https://github.com/harryli0088/sourdough-calculator">https://github.com/harryli0088/sourdough-calculator</Blanchor></p>
  <p>Icons provided by Font Awesome: <Blanchor href="https://fontawesome.com/license">https://fontawesome.com/license</Blanchor></p>
</footer>

<style>
	footer {
		background-color: #ddd;
	}
</style>

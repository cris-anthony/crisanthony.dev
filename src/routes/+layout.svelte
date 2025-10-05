<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';
	import { Copyright } from '$lib';
	
	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<style>
	.main {
		min-height: 100vh;
		background-color: var(--color-bg);
		color: var(--color-text);
	}
</style>

<main class="main p-8 md:p-24">
	{@render children?.()}
	<Copyright />
</main>


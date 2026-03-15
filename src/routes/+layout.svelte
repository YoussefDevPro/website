<script lang="ts">
	export const prerender = true;
	export const ssr = false;

	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
	import '../app.css';
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children()}

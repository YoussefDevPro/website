<script lang="ts">
	import { onMount } from 'svelte';
	import { blur } from 'svelte/transition';

	let { children, startDelay = 0, class: className = '' } = $props();

	let visible = $state(false);

	onMount(() => {
		const timer = setTimeout(() => {
			visible = true;
		}, startDelay);

		return () => clearTimeout(timer);
	});
</script>

{#if visible}
	<div in:blur={{ duration: 800 }} class={className}>
		{@render children()}
	</div>
{/if}

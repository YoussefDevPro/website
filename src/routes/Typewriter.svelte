<script lang="ts">
	import { onMount } from 'svelte';
	import { blur } from 'svelte/transition';

	let { text, startDelay = 0, speed = 30, longSpeed = 300, class: className = '' } = $props();

	let displayedText = $state('');
	const pauseChars = ['|', '!', '.', ',', '?', ':', ';'];

	onMount(() => {
		let i = 0;
		let timeoutId: number;

		function type() {
			if (i < text.length) {
				const char = text.charAt(i);
				let currentDelay = speed;

				if (pauseChars.includes(char)) {
					currentDelay = longSpeed;
					if (char !== '|') displayedText += char;
				} else {
					displayedText += char;
				}

				i++;
				timeoutId = setTimeout(type, currentDelay);
			}
		}

		// Start typing after the initial delay
		const startTimeout = setTimeout(type, startDelay);

		return () => {
			clearTimeout(startTimeout);
			clearTimeout(timeoutId);
		};
	});
</script>

<p class={className}>
	{#each displayedText as char}
		<span in:blur={{ duration: 400 }}>{char}</span>
	{/each}
</p>

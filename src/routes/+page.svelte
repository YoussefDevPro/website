<script lang="ts">
	import { onMount } from 'svelte';
	import { blur, fly } from 'svelte/transition';
	import Typewriter from './Typewriter.svelte';

	const fullText = 'Hai! My name is';
	const Name = 'Yousafe!';
	let displayedText = $state('');
	let displayerName = $state('');
	let speed = 50;
	const longSpeed = 400;
	const pauseChars = ['|', '!', '.', ',', '?'];

	onMount(() => {
		let i = 0;
		let timeoutId: number;

		function type() {
			if (i < fullText.length) {
				const char = fullText.charAt(i);
				let currentDelay = speed;

				if (pauseChars.includes(char)) {
					currentDelay = longSpeed;
					if (char !== '|') displayedText += char;
				} else {
					displayedText += char;
				}

				i++;
				timeoutId = setTimeout(type, currentDelay);
			} else if (i < fullText.length + Name.length) {
				const char = Name.charAt(i - fullText.length);
				displayerName += char;
				i++;
				timeoutId = setTimeout(type, speed);
			}
		}

		type(); // Start the loop

		return () => clearTimeout(timeoutId);
	});
</script>

<div class="m-32">
	<h1 class="text-7xl mb-5 gap-4">
		{#each displayedText as char}
			<span in:blur={{ duration: 500 }}>
				{char === '' ? '' : char}
			</span>
		{/each}

		{#each displayerName as char}
			<span in:fly={{ y: 20, duration: 800 }} class="inline-block">
				<span in:blur={{ duration: 800 }} class="inline-block">
					{char === ' ' ? '\u00A0' : char}
				</span>
			</span>
		{/each}
	</h1>
	<p class="text-3xl">I'm a 15yo <span class="italic">(soon 16yo)</span> dev from morrocco.</p>
	<p class="text-3xl">
		Im a <span class="rusty-text"><i class="nf nf-dev-rust"></i> rusty developper</span>, and i code
		a lot of backend or nerdy stuff.
	</p>
	<p class="text-3xl">
		and im also a <span class="text-[#FCF434]">non</span>bin<span class="text-[#9C59D1]">ary</span>
		<span class="text-[#A3A3A3]">ase</span>xu<span class="text-[#800080]">al</span> furry
		<i class="nf nf-md-cat"></i>.
	</p>
	<p class="text-3xl">I play undertale/deltarune a LOT.</p>
	<p class="text-3xl">And this is my very first website OwO.</p>
</div>

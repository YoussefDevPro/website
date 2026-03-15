<script lang="ts">
	import { onMount } from 'svelte';
	import { blur, fly } from 'svelte/transition';
	import Typewriter from './Typewriter.svelte';

	const fullText = 'Hai! My name is';
	const Name = 'Yousafe!';
	let displayedText = $state('');
	let displayerName = $state('');
	let speed = 50;
	const longSpeed = 600;
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
	<Typewriter startDelay={2000} class="text-3xl">
		<p>I'm a 15yo <span class="italic">(soon 16yo)</span> dev from Morocco.</p>
	</Typewriter>

	<Typewriter startDelay={3000} class="text-3xl">
		<p>
			I'm a <span class="rusty-text"><i class="nf nf-dev-rust"></i> rusty developer</span>, and I
			code a lot of backend or nerdy stuff.
		</p>
	</Typewriter>

	<Typewriter startDelay={4000} class="text-3xl">
		<p>
			And I'm also a
			<span class="text-[#FCF434]">non</span>bin<span class="text-[#9C59D1]">ary</span>
			<span class="text-[#A3A3A3]">ase</span>xu<span class="text-[#800080]">al</span>
			furry <i class="nf nf-md-cat"></i>.
		</p>
	</Typewriter>

	<Typewriter startDelay={5000} class="text-3xl">
		<p>I play Undertale/Deltarune a LOT.</p>
	</Typewriter>

	<Typewriter startDelay={6500} class="text-3xl">
		<p>
			And this is my <span class="p-1 pixel-corners inline-flex align-middle"
				><img src="github.svg" alt="guthib" class="m-0 w-7 mr-2 inline" /> guthib</span
			> if you ever wanna check :3
		</p>
	</Typewriter>
</div>

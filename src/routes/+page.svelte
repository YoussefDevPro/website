<script lang="ts">
	import { onMount } from 'svelte';
	import { blur, fly } from 'svelte/transition';
	import Typewriter from './Typewriter.svelte';

	type Button = {
		name: string;
		icon: string;
		link: string;
	};

	let buttons: Button[] = [
		{ name: 'blog', icon: 'nf-fa-file_pen', link: '/soontm' },
		{ name: 'projects', icon: 'nf-custom-folder_github', link: '/soontm' }
	];

	let guthibProfile = 'https://github.com/YoussefDevPro';

	let currentYear = new Date().getFullYear();

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

		type();

		return () => clearTimeout(timeoutId);
	});
</script>

<div class="flex h-screen flex-col overflow-hidden">
	<div class=" gap-2 bg-white pt-1 flex place-content-center">
		{#each buttons as button}
			<a href={button.link}
				><button class="p-2 pixel-corners text-2xl bg-black"
					><i class="nf {button.icon}"></i> {button.name}</button
				></a
			>
		{/each}
	</div>
	<div class="bg-white flex-1">
		<div class="pixel-corners-2 m-0 p-0 bg-black h-full">
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
				<Typewriter startDelay={2500} class="text-3xl">
					<p>I'm a 15yo <span class="italic">(soon 16yo)</span> dev from Morocco.</p>
				</Typewriter>
				<p></p>

				<Typewriter startDelay={4000} class="text-3xl">
					<p>
						I'm a <span class="rusty-text"><i class="nf nf-dev-rust"></i> rusty developer</span>,
						and I code a lot of backend and nerdy stuff.
					</p>
				</Typewriter>
				<p></p>

				<Typewriter startDelay={7000} class="text-3xl">
					<p>
						And I'm also a
						<span class="text-[#FCF434]">non</span>bin<span class="text-[#9C59D1]">ary</span>
						<span class="text-[#A3A3A3]">ase</span>xu<span class="text-[#800080]">al</span>
						furry <i class="nf nf-md-cat"></i>.
					</p>
				</Typewriter>
				<p></p>

				<Typewriter startDelay={8500} class="text-3xl">
					<p>I play Undertale and Deltarune a LOT.</p>
				</Typewriter>
				<!-- no idea why i have to do this  -->
				<p></p>

				<Typewriter startDelay={10500} class="text-3xl">
					<p>
						And this is my <a href={guthibProfile} target="_blank"
							><button class="p-1 pixel-corners inline-flex align-middle"
								><img src="github.svg" alt="guthib" class="m-0 w-7 mr-2 pixelated inline" /> guthib</button
							>
						</a> if you ever wanna check :3
					</p>
				</Typewriter>
			</div>
		</div>
	</div>
	<div class="bg-white p-1 pl-10">
		<p class="text-black date text-xl">{currentYear}-{currentYear + 1}</p>
	</div>
</div>

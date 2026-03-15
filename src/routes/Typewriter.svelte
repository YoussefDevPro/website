<script lang="ts">
	import { onMount } from 'svelte';

	let { children, startDelay = 0, speed = 1, class: className = '' } = $props();
	let visible = $state(false);

	function typewriter(node: HTMLElement, { speed = 1 }) {
		const textNodes: { node: Text; fullText: string; start: number; end: number }[] = [];
		const elementNodes: { node: HTMLElement; start: number }[] = [];
		let totalLength = 0;

		function walk(current: Node) {
			if (current.nodeType === Node.TEXT_NODE) {
				const text = current.textContent || '';
				textNodes.push({
					node: current as Text,
					fullText: text,
					start: totalLength,
					end: totalLength + text.length
				});
				totalLength += text.length;
			} else if (current.nodeType === Node.ELEMENT_NODE) {
				const el = current as HTMLElement;
				elementNodes.push({ node: el, start: totalLength });

				el.style.opacity = '0';
				el.style.transform = 'scale(0.5)';
				el.style.transition = 'all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275)';
				el.style.display = 'inline-block'; // Required for transform

				el.childNodes.forEach(walk);
			}
		}

		walk(node);

		return {
			duration: totalLength / (speed * 0.03),
			tick: (t: number) => {
				const currentVisible = Math.floor(t * totalLength);

				for (const { node, fullText, start } of textNodes) {
					const visibleChars = Math.max(0, Math.min(fullText.length, currentVisible - start));
					node.textContent = fullText.slice(0, visibleChars);
				}

				for (const { node, start } of elementNodes) {
					if (currentVisible >= start) {
						node.style.opacity = '1';
						node.style.transform = 'scale(1)';
					} else {
						node.style.opacity = '0';
						node.style.transform = 'scale(0.5)';
					}
				}
			}
		};
	}

	onMount(() => {
		const timer = setTimeout(() => {
			visible = true;
		}, startDelay);
		return () => clearTimeout(timer);
	});
</script>

{#if visible}
	<div in:typewriter={{ speed }} class={className}>
		{@render children()}
	</div>
{/if}

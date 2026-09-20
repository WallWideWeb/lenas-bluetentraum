<script lang="ts">
	import './layout.css';
	import { onMount } from 'svelte';

	let { children } = $props();

	onMount(() => {
		let cleanup = () => {};

		(async () => {
			const [{ default: Lenis }, { default: gsap }, { ScrollTrigger }] = await Promise.all([
				import('lenis'),
				import('gsap'),
				import('gsap/ScrollTrigger')
			]);

			gsap.registerPlugin(ScrollTrigger);

			const lenis = new Lenis({
				duration: 1.2,
				easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
				smoothWheel: true,
				// Touch-Gesten bleiben nativ (kein Sync/"Hijacking"), damit Scrollen auf
				// Mobilgeräten sich weiterhin natürlich und unverzögert anfühlt.
				syncTouch: false,
				touchMultiplier: 1
			});

			lenis.on('scroll', ScrollTrigger.update);

			function raf(time: number) {
				lenis.raf(time * 1000);
			}

			gsap.ticker.add(raf);
			gsap.ticker.lagSmoothing(0);

			cleanup = () => {
				gsap.ticker.remove(raf);
				lenis.destroy();
			};
		})();

		return () => cleanup();
	});
</script>

{@render children()}

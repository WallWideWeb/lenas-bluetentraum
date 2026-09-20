<script lang="ts">
	type World = {
		kicker: string;
		title: string;
		description: string;
		image: string;
		href: string;
	};

	const worlds: World[] = [
		{
			kicker: 'Atelier',
			title: 'Atelier & Garn',
			description: 'Schneiderei, Maßanfertigungen und Handwerk mit Liebe zum Detail.',
			image:
				'https://images.unsplash.com/photo-1556905055-8f358a7a47b2?auto=format&fit=crop&w=1200&q=80',
			href: '/atelier-garn'
		},
		{
			kicker: 'Floristik',
			title: 'Floristik & Blüten',
			description: 'Hochzeitsfloristik, Eventdekoration und zeitlose Trockenblumen.',
			image:
				'https://images.unsplash.com/photo-1487530811176-3780de880c2d?auto=format&fit=crop&w=1200&q=80',
			href: '/floristik-blueten'
		}
	];

	let headerVisible = $state(false);

	let headerEl: HTMLElement;
	let heroEl: HTMLElement;
	let glowEl: HTMLDivElement;
	let logoWrapEl: HTMLDivElement;
	let logoEl: HTMLImageElement;
	let aboutEl: HTMLDivElement;
	let aboutImageWrapEl: HTMLDivElement;
	let aboutImageEl: HTMLImageElement;
	let aboutTextEl: HTMLDivElement;
	let curtainEl: HTMLDivElement;
	let scrollHintEl: HTMLDivElement;
	let scrollLineEl: HTMLSpanElement;
	let splitEl: HTMLElement;
	let splitHeadingEl: HTMLDivElement;
	let mobileCards: HTMLAnchorElement[] = $state([]);
	let desktopPanels: HTMLAnchorElement[] = $state([]);

	$effect(() => {
		// eslint-disable-next-line @typescript-eslint/no-explicit-any
		let ctx: any;
		// eslint-disable-next-line @typescript-eslint/no-explicit-any
		let mm: any;

		(async () => {
			const [{ default: gsap }, { ScrollTrigger }] = await Promise.all([
				import('gsap'),
				import('gsap/ScrollTrigger')
			]);
			gsap.registerPlugin(ScrollTrigger);

			ctx = gsap.context(() => {
				const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

				gsap.set(curtainEl, { yPercent: 100 });
				gsap.set(aboutImageWrapEl, { opacity: 0, scale: 0.95, y: 28 });
				gsap.set(aboutTextEl, { opacity: 0, y: 20 });

				if (reduceMotion) {
					gsap.set([logoWrapEl, scrollHintEl], { opacity: 1, y: 0, scale: 1 });
				} else {
					gsap.from(logoWrapEl, {
						opacity: 0,
						scale: 0.92,
						y: 18,
						duration: 1.2,
						delay: 0.15,
						ease: 'power2.out',
						clearProps: 'opacity,transform'
					});
					gsap.from(scrollHintEl, {
						opacity: 0,
						y: 10,
						duration: 0.9,
						delay: 0.9,
						ease: 'power2.out'
					});
				}

				// eslint-disable-next-line @typescript-eslint/no-explicit-any
				const setupScene = (isMobile: boolean) => {
					// eslint-disable-next-line @typescript-eslint/no-explicit-any
					let glowPulse: any;
					// eslint-disable-next-line @typescript-eslint/no-explicit-any
					let scrollLinePulse: any;

					if (!reduceMotion) {
						glowPulse = gsap.to(glowEl, {
							opacity: isMobile ? 0.65 : 0.85,
							scale: isMobile ? 1.04 : 1.1,
							duration: isMobile ? 4 : 3.2,
							repeat: -1,
							yoyo: true,
							ease: 'sine.inOut'
						});

						scrollLinePulse = gsap.fromTo(
							scrollLineEl,
							{ yPercent: -100 },
							{ yPercent: 100, duration: 1.6, repeat: -1, ease: 'power1.inOut' }
						);
					}

					if (reduceMotion) {
						// No pin / fly-through for reduced-motion users: just reveal the
						// sticky header once the hero has scrolled past.
						ScrollTrigger.create({
							trigger: heroEl,
							start: 'bottom top',
							onEnter: () => (headerVisible = true),
							onLeaveBack: () => (headerVisible = false)
						});
						if (splitHeadingEl) {
							gsap.set(splitHeadingEl, { opacity: 1, y: 0 });
						}
						return;
					}

					const flyTl = gsap.timeline({
						scrollTrigger: {
							trigger: heroEl,
							start: 'top top',
							end: '+=75%',
							pin: true,
							scrub: 1,
							anticipatePin: 1
						},
						onStart: () => {
							glowPulse?.kill();
							scrollLinePulse?.kill();
						}
					});

					flyTl
						// 0% – 40%: Logo + Glow fliegen weg, kein Leerlauf danach.
						.to(scrollHintEl, { opacity: 0, y: 8, duration: 0.05, ease: 'none' }, 0)
						.to(
							logoEl,
							{
								scale: isMobile ? 2.2 : 2.6,
								yPercent: -120,
								opacity: 0,
								ease: 'power1.in',
								duration: 0.38
							},
							0.02
						)
						.to(
							glowEl,
							{
								scale: isMobile ? 2.4 : 3.2,
								opacity: 0,
								ease: 'none',
								duration: 0.4
							},
							0
						)
						// 25% – 60%: "Über Lena"-Szene blendet synchron ein.
						.fromTo(
							aboutImageWrapEl,
							{ opacity: 0, scale: 0.95, y: 28 },
							{ opacity: 1, scale: 1, y: 0, ease: 'power2.out', duration: 0.35 },
							0.25
						)
						.fromTo(
							aboutTextEl,
							{ opacity: 0, y: 20 },
							{ opacity: 1, y: 0, ease: 'power2.out', duration: 0.32 },
							0.28
						)
						// 60% – 85%: die Szene bleibt einfach stehen (keine Tweens nötig).
						// 80% – 100%: Split-Section dockt an, Lena-Szene gleitet weich raus.
						.to(
							aboutEl,
							{ opacity: 0, y: -40, ease: 'power1.in', duration: 0.2 },
							0.8
						)
						.to(
							curtainEl,
							{ yPercent: 0, ease: 'power2.inOut', duration: 0.2 },
							0.8
						)
						.to(
							headerEl,
							{
								opacity: 1,
								duration: 0.15,
								ease: 'power1.out',
								onStart: () => (headerVisible = true),
								onReverseComplete: () => (headerVisible = false)
							},
							0.85
						);

					if (!isMobile) {
						flyTl.fromTo(
							aboutImageEl,
							{ filter: 'blur(14px)' },
							{ filter: 'blur(0px)', ease: 'power2.out', duration: 0.35 },
							0.25
						);
					}

					if (splitHeadingEl) {
						gsap.from(splitHeadingEl, {
							opacity: 0,
							y: isMobile ? 16 : 24,
							duration: isMobile ? 0.6 : 0.9,
							ease: 'power2.out',
							scrollTrigger: {
								trigger: splitEl,
								start: 'top 82%'
							}
						});
					}

					const revealTargets = isMobile ? mobileCards : desktopPanels;
					revealTargets.forEach((el, i) => {
						if (!el) return;
						gsap.from(el, {
							opacity: 0,
							y: isMobile ? 24 : 32,
							duration: isMobile ? 0.6 : 0.9,
							delay: isMobile ? i * 0.08 : i * 0.12,
							ease: 'power2.out',
							scrollTrigger: {
								trigger: el,
								start: 'top 88%'
							}
						});
					});
				};

				mm = gsap.matchMedia();
				mm.add('(max-width: 767px)', () => setupScene(true));
				mm.add('(min-width: 768px)', () => setupScene(false));
			});
		})();

		return () => {
			mm?.revert();
			ctx?.revert();
		};
	});
</script>

<header
	bind:this={headerEl}
	aria-hidden={!headerVisible}
	class="fixed inset-x-0 top-0 z-50 flex items-center justify-center gap-3 border-b border-ink/10 bg-background/70 px-6 py-3 opacity-0 backdrop-blur-md"
>
	<img src="/logo.jpg" alt="" class="h-8 w-auto" />
	<span class="font-serif text-sm tracking-wide text-ink sm:text-base">Lena's Garn &amp; Blütentraum</span>
</header>

<main>
	<section
		bind:this={heroEl}
		id="hero"
		class="relative flex h-svh min-h-svh w-full flex-col items-center justify-center overflow-hidden bg-background"
	>
		<div
			bind:this={glowEl}
			aria-hidden="true"
			style="will-change: transform, opacity;"
			class="pointer-events-none absolute left-1/2 top-1/2 h-[60vmax] w-[60vmax] -translate-x-1/2 -translate-y-1/2 rounded-full bg-accent/40 opacity-50 blur-3xl"
		></div>

		<div bind:this={logoWrapEl}>
			<img
				bind:this={logoEl}
				src="/logo.jpg"
				alt="Lena's Garn &amp; Blütentraum"
				style="will-change: transform, opacity;"
				class="relative z-10 w-[85vw] max-h-[75vh] object-contain mix-blend-multiply md:h-[80vh] md:w-auto md:max-w-[90vw]"
			/>
		</div>

		<div
			bind:this={scrollHintEl}
			class="absolute bottom-8 left-1/2 z-10 flex -translate-x-1/2 flex-col items-center gap-3 text-ink/60"
		>
			<span class="text-[0.65rem] font-medium uppercase tracking-[0.35em]">Entdecken</span>
			<span class="relative block h-10 w-px overflow-hidden bg-ink/15">
				<span bind:this={scrollLineEl} class="absolute inset-x-0 top-0 h-full w-full bg-accent"
				></span>
			</span>
		</div>

		<div
			bind:this={aboutEl}
			aria-hidden="true"
			style="will-change: transform, opacity;"
			class="pointer-events-none absolute inset-0 z-[15] flex flex-col items-center justify-center gap-8 px-6 py-12 sm:px-10 md:flex-row md:gap-16 md:px-16 lg:gap-24"
		>
			<div
				bind:this={aboutImageWrapEl}
				style="will-change: transform, opacity;"
				class="relative w-[64vw] max-w-[300px] shrink-0 md:w-[32vw] md:max-w-[380px]"
			>
				<div
					aria-hidden="true"
					class="absolute -inset-6 -z-10 rounded-[2.5rem] bg-accent/25 blur-3xl"
				></div>
				<img
					bind:this={aboutImageEl}
					src="/lena.jpg"
					alt="Portrait von Lena"
					style="object-position: top center; will-change: filter;"
					class="aspect-[4/5] w-full rounded-[2rem] object-cover shadow-[0_30px_70px_-25px_rgba(28,29,31,0.4)]"
				/>
			</div>

			<div
				bind:this={aboutTextEl}
				style="will-change: transform, opacity;"
				class="max-w-md text-center md:text-left"
			>
				<h2 class="font-serif text-3xl text-ink sm:text-4xl">Mit Liebe zum Handwerk</h2>
				<p class="mt-4 text-base leading-relaxed text-ink/75 sm:text-lg">
					Wo feine Garne, präzise Schnitte und lebendige Blüten zu unverwechselbaren Momenten
					verschmelzen. Willkommen in meinem Traum.
				</p>
				<span class="mt-6 inline-block font-serif text-2xl italic text-accent">Lena</span>
			</div>
		</div>

		<div
			bind:this={curtainEl}
			aria-hidden="true"
			style="will-change: transform;"
			class="pointer-events-none absolute inset-0 z-20 rounded-t-[3rem] bg-linear-to-b from-accent/20 via-nude/30 to-background shadow-[0_-20px_60px_-15px_rgba(28,29,31,0.25)]"
		></div>
	</section>

	<section bind:this={splitEl} id="welten" class="relative bg-background">
		<div bind:this={splitHeadingEl} class="mx-auto max-w-2xl px-6 pb-10 pt-16 text-center sm:pt-24">
			<h2 class="font-serif text-2xl text-ink sm:text-3xl">Zwei Welten, eine Leidenschaft</h2>
			<p class="mt-3 text-sm text-ink/70 sm:text-base">
				Von zarten Garnen bis zu duftenden Blüten – entdecken Sie, wofür Lena brennt.
			</p>
		</div>

		<div class="flex flex-col gap-5 px-4 pb-16 sm:px-6 md:hidden">
			{#each worlds as world, i}
				<a
					bind:this={mobileCards[i]}
					href={world.href}
					class="group relative block aspect-3/4 w-full overflow-hidden rounded-3xl shadow-lg transition-transform duration-300 active:scale-[0.98]"
				>
					<img
						src={world.image}
						alt=""
						loading="lazy"
						class="absolute inset-0 h-full w-full object-cover transition-transform duration-700 group-active:scale-105"
					/>
					<div class="absolute inset-0 bg-linear-to-t from-ink/80 via-ink/20 to-transparent"></div>
					<div class="relative flex h-full flex-col justify-end gap-2 p-6 text-background">
						<span class="text-xs uppercase tracking-[0.3em] text-background/80">{world.kicker}</span>
						<h3 class="font-serif text-2xl">{world.title}</h3>
						<p class="text-sm text-background/85">{world.description}</p>
						<span class="mt-2 inline-flex items-center gap-2 text-sm font-medium">
							Mehr erfahren
							<span class="transition-transform duration-300 group-active:translate-x-1">→</span>
						</span>
					</div>
				</a>
			{/each}
		</div>

		<div class="split-wrap relative hidden h-[85vh] w-full md:flex">
			{#each worlds as world, i}
				<a
					bind:this={desktopPanels[i]}
					href={world.href}
					class="split-panel group relative flex-1 overflow-hidden"
				>
					<img src={world.image} alt="" loading="lazy" class="split-img absolute inset-0 h-full w-full object-cover" />
					<div class="absolute inset-0 bg-linear-to-t from-ink/70 via-ink/10 to-transparent"></div>
					<div class="relative flex h-full flex-col items-center justify-end gap-3 p-10 text-center text-background lg:p-14">
						<span class="text-xs uppercase tracking-[0.35em] text-background/80">{world.kicker}</span>
						<h3 class="font-serif text-3xl lg:text-4xl">{world.title}</h3>
						<p class="max-w-sm text-sm text-background/85 lg:text-base">{world.description}</p>
						<span class="mt-2 inline-flex items-center gap-2 text-sm font-medium">
							Mehr erfahren
							<span class="transition-transform duration-300 group-hover:translate-x-1">→</span>
						</span>
					</div>
				</a>
			{/each}
		</div>
	</section>
</main>

<style>
	.split-panel {
		flex: 1;
		transition:
			flex 700ms cubic-bezier(0.16, 1, 0.3, 1),
			filter 700ms ease;
	}

	.split-wrap:hover .split-panel:not(:hover) {
		flex: 0.82;
		filter: brightness(0.68) saturate(0.9);
	}

	.split-panel:hover {
		flex: 1.28;
	}

	.split-img {
		transition: transform 900ms cubic-bezier(0.16, 1, 0.3, 1);
	}

	.split-panel:hover .split-img {
		transform: scale(1.06);
	}

	@media (prefers-reduced-motion: reduce) {
		.split-panel,
		.split-img {
			transition: none;
		}
	}
</style>

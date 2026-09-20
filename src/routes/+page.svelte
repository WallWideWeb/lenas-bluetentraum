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

	const synergyPoints = [
		{
			title: 'Harmonische Farbkonzepte',
			description:
				'Garne und Blüten werden aufeinander abgestimmt, damit Kleidung und Dekoration eine gemeinsame Sprache sprechen.'
		},
		{
			title: 'Alles aus einer Hand',
			description:
				'Von der Maßanfertigung bis zum Brautstrauß – ein Ansprechpartner begleitet Ihr Projekt von Anfang bis Ende.'
		},
		{
			title: 'Liebe zum kleinsten Detail',
			description:
				'Jeder Saum, jede Blüte wird mit derselben Sorgfalt gefertigt – für Momente, die man fühlt statt nur sieht.'
		}
	];

	type LookbookItem = { badge: string; image: string; alt: string };
	const lookbook: LookbookItem[] = [
		{
			badge: 'Atelier',
			image: 'https://images.unsplash.com/photo-1556905055-8f358a7a47b2?auto=format&fit=crop&w=900&q=80',
			alt: 'Handgestrickte Wollwaren in warmen Tönen'
		},
		{
			badge: 'Floristik',
			image: 'https://images.unsplash.com/photo-1487530811176-3780de880c2d?auto=format&fit=crop&w=900&q=80',
			alt: 'Üppiger Hochzeitsstrauß mit Rosen'
		},
		{
			badge: 'Atelier',
			image: 'https://images.unsplash.com/photo-1445205170230-053b83016050?auto=format&fit=crop&w=900&q=80',
			alt: 'Kuratierte Kleidungsstücke im Atelier'
		},
		{
			badge: 'Floristik',
			image: 'https://images.unsplash.com/photo-1487070183336-b863922373d4?auto=format&fit=crop&w=900&q=80',
			alt: 'Blumenstand mit frischen Sträußen'
		},
		{
			badge: 'Atelier',
			image: 'https://images.unsplash.com/photo-1601924994987-69e26d50dc26?auto=format&fit=crop&w=900&q=80',
			alt: 'Farbenfrohe Stoffe auf dem Kleiderbügel'
		},
		{
			badge: 'Floristik',
			image: 'https://images.unsplash.com/photo-1519378058457-4c29a0a2efac?auto=format&fit=crop&w=900&q=80',
			alt: 'Nahaufnahme leuchtend roter Blüten'
		}
	];

	let lookbookScrollEl: HTMLDivElement;
	function scrollLookbook(direction: 1 | -1) {
		if (!lookbookScrollEl) return;
		lookbookScrollEl.scrollBy({ left: lookbookScrollEl.clientWidth * 0.8 * direction, behavior: 'smooth' });
	}

	const interests = ['Braut & Hochzeit', 'Maßanfertigung', 'Florales Design'] as const;
	let selectedInterest: (typeof interests)[number] = $state(interests[0]);

	const phoneDisplay = '+49 151 46159350';
	const phoneHref = 'tel:+4915146159350';
	const email = 'info@lenas-bluetentraum.de';
	const address = 'Schulstraße 28, 57636 Mammelzen';
	const hours = 'Täglich 8:00–18:00 Uhr und nach Vereinbarung';
	const whatsappHref = $derived(
		`https://wa.me/4915146159350?text=${encodeURIComponent(`Hallo Lena, ich interessiere mich für: ${selectedInterest}`)}`
	);
	const emailHref = $derived(`mailto:${email}?subject=${encodeURIComponent(`Anfrage: ${selectedInterest}`)}`);
	const currentYear = new Date().getFullYear();

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
						// Phase 1 (0% – 35%): Logo + Glow fliegen cineastisch weg.
						.to(scrollHintEl, { opacity: 0, y: 8, duration: 0.04, ease: 'none' }, 0)
						.to(
							logoEl,
							{
								scale: isMobile ? 2.2 : 2.6,
								yPercent: -130,
								opacity: 0,
								ease: 'power1.in',
								duration: 0.33
							},
							0.02
						)
						.to(
							glowEl,
							{
								scale: isMobile ? 2.4 : 3.2,
								opacity: 0,
								ease: 'none',
								duration: 0.35
							},
							0
						)
						// Phase 2 (25% – 60%): "Über Lena"-Szene blendet synchron ein.
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
						// Stille Haltephase bis 60% (keine Tweens nötig).
						// Phase 3 (60% – 100%): Split-Section dockt an, Lena-Szene gleitet
						// weich raus – über die volle Distanz, damit nie leerer Hintergrund steht.
						.to(
							aboutEl,
							{ opacity: 0, y: -50, ease: 'power1.in', duration: 0.4 },
							0.6
						)
						.to(
							curtainEl,
							{ yPercent: 0, ease: 'power2.inOut', duration: 0.4 },
							0.6
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

<svelte:head>
	<title>Lena's Garn &amp; Blütentraum</title>
	<meta
		name="description"
		content="Schneiderei und Floristik aus einer Hand: Maßanfertigungen, Hochzeitsfloristik und Trockenblumen aus Mammelzen."
	/>
</svelte:head>

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
					class="aspect-[4/5] w-full rounded-[3rem_1.25rem_3rem_1.25rem] object-cover shadow-[0_35px_70px_-25px_rgba(180,130,95,0.45)]"
				/>
			</div>

			<div
				bind:this={aboutTextEl}
				style="will-change: transform, opacity;"
				class="max-w-md text-center md:text-left"
			>
				<h2 class="font-serif text-3xl text-ink sm:text-4xl">Handwerk mit Seele</h2>
				<p class="mt-4 text-base leading-relaxed text-ink/75 sm:text-lg">
					„Wo feine Garne und lebendige Blüten zu einer gemeinsamen Geschichte verschmelzen.“
				</p>
				<span class="mt-6 inline-block font-serif text-2xl italic text-accent">— Lena</span>
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

	<section class="bg-background px-6 py-20 sm:px-10 sm:py-28">
		<div class="mx-auto max-w-5xl text-center">
			<span class="text-xs uppercase tracking-[0.35em] text-accent">Warum beides zusammengehört</span>
			<h2 class="mt-3 font-serif text-3xl text-ink sm:text-4xl">Das Synergie-Prinzip</h2>
		</div>

		<div class="mx-auto mt-14 grid max-w-5xl gap-10 sm:grid-cols-3">
			{#each synergyPoints as point, i}
				<div class="text-center">
					<span
						class="mx-auto flex h-12 w-12 items-center justify-center rounded-full border border-accent/30 font-serif text-lg text-accent"
					>
						{i + 1}
					</span>
					<h3 class="mt-5 font-serif text-xl text-ink">{point.title}</h3>
					<p class="mt-3 text-sm leading-relaxed text-ink/70">{point.description}</p>
				</div>
			{/each}
		</div>
	</section>

	<section class="bg-nude/10 py-20 sm:py-28">
		<div class="mx-auto flex max-w-6xl items-end justify-between gap-6 px-6 sm:px-10">
			<div>
				<span class="text-xs uppercase tracking-[0.35em] text-accent">Lookbook</span>
				<h2 class="mt-3 font-serif text-3xl text-ink sm:text-4xl">Ein Blick in unsere Welt</h2>
			</div>
			<div class="hidden shrink-0 gap-3 sm:flex">
				<button
					type="button"
					aria-label="Zurück"
					onclick={() => scrollLookbook(-1)}
					class="flex h-11 w-11 items-center justify-center rounded-full border border-ink/15 text-ink transition-colors hover:bg-ink hover:text-background"
				>
					←
				</button>
				<button
					type="button"
					aria-label="Weiter"
					onclick={() => scrollLookbook(1)}
					class="flex h-11 w-11 items-center justify-center rounded-full border border-ink/15 text-ink transition-colors hover:bg-ink hover:text-background"
				>
					→
				</button>
			</div>
		</div>

		<div
			bind:this={lookbookScrollEl}
			class="mt-10 flex snap-x snap-mandatory gap-5 overflow-x-auto px-6 pb-4 sm:px-10 [&::-webkit-scrollbar]:hidden"
			style="scrollbar-width: none;"
		>
			{#each lookbook as item}
				<div
					class="group relative aspect-4/5 w-[75vw] max-w-[320px] shrink-0 snap-start overflow-hidden rounded-2xl sm:w-[320px]"
				>
					<img
						src={item.image}
						alt={item.alt}
						loading="lazy"
						class="absolute inset-0 h-full w-full object-cover transition-transform duration-700 group-hover:scale-110"
					/>
					<div class="absolute inset-0 bg-linear-to-t from-ink/50 via-transparent to-transparent"></div>
					<span
						class="absolute left-4 top-4 rounded-full bg-background/85 px-3 py-1 text-xs font-medium uppercase tracking-[0.2em] text-ink backdrop-blur-sm"
					>
						{item.badge}
					</span>
				</div>
			{/each}
		</div>
	</section>

	<section class="relative overflow-hidden bg-linear-to-b from-accent/10 via-background to-background px-6 py-20 sm:px-10 sm:py-28">
		<div
			class="mx-auto max-w-2xl rounded-[2.5rem] border border-white/40 bg-background/60 p-8 text-center shadow-[0_40px_80px_-40px_rgba(28,29,31,0.35)] backdrop-blur-xl sm:p-14"
		>
			<span class="text-xs uppercase tracking-[0.35em] text-accent">Termin &amp; Anfrage</span>
			<h2 class="mt-3 font-serif text-3xl text-ink sm:text-4xl">Lass uns deine Idee verwirklichen.</h2>
			<p class="mt-4 text-sm text-ink/70 sm:text-base">
				Wähle, worum es geht – dann melden wir uns über den passenden Kontaktweg.
			</p>

			<div class="mt-8 flex flex-wrap justify-center gap-3">
				{#each interests as interest}
					<button
						type="button"
						onclick={() => (selectedInterest = interest)}
						aria-pressed={selectedInterest === interest}
						class="rounded-full border px-5 py-2 text-sm font-medium transition-colors {selectedInterest ===
						interest
							? 'border-accent bg-accent text-background'
							: 'border-ink/15 text-ink hover:border-accent/40'}"
					>
						{interest}
					</button>
				{/each}
			</div>

			<div class="mt-10 flex flex-col items-center justify-center gap-3 sm:flex-row">
				<a
					href={whatsappHref}
					target="_blank"
					rel="noopener noreferrer"
					class="inline-flex items-center gap-2 rounded-full bg-accent px-6 py-3 text-sm font-medium text-background transition-colors hover:bg-accent-light"
				>
					WhatsApp schreiben
				</a>
				<a
					href={emailHref}
					class="inline-flex items-center gap-2 rounded-full border border-ink/20 px-6 py-3 text-sm font-medium text-ink transition-colors hover:bg-ink hover:text-background"
				>
					Per E-Mail anfragen
				</a>
			</div>

			<p class="mt-5 text-sm text-ink/60">
				Oder direkt anrufen: <a href={phoneHref} class="font-medium text-ink hover:text-accent">{phoneDisplay}</a>
			</p>
		</div>
	</section>

	<footer class="border-t border-ink/10 bg-ink text-background">
		<div class="mx-auto grid max-w-6xl gap-12 px-6 py-16 sm:grid-cols-3 sm:px-10">
			<div>
				<div class="flex items-center gap-3">
					<span class="flex h-10 w-10 items-center justify-center overflow-hidden rounded-full bg-background p-1">
						<img src="/logo.jpg" alt="" class="h-full w-full rounded-full object-cover" />
					</span>
					<span class="font-serif text-lg tracking-wide">Lena's Garn &amp; Blütentraum</span>
				</div>
				<p class="mt-4 text-sm text-background/70">
					Schneiderei und Floristik aus einer Hand – mit Liebe zum Handwerk aus Mammelzen.
				</p>
				<a
					href="https://instagram.com"
					target="_blank"
					rel="noopener noreferrer"
					class="mt-5 inline-flex items-center gap-2 text-sm text-background/70 transition-colors hover:text-background"
				>
					Instagram
				</a>
			</div>

			<div>
				<h3 class="text-xs uppercase tracking-[0.3em] text-background/50">Kontakt</h3>
				<ul class="mt-4 space-y-2 text-sm text-background/80">
					<li>{address}</li>
					<li><a href={phoneHref} class="hover:text-background">{phoneDisplay}</a></li>
					<li><a href="mailto:{email}" class="hover:text-background">{email}</a></li>
				</ul>
			</div>

			<div>
				<h3 class="text-xs uppercase tracking-[0.3em] text-background/50">Atelierzeiten</h3>
				<p class="mt-4 text-sm text-background/80">{hours}</p>
				<div class="mt-6 flex gap-4 text-sm text-background/60">
					<a href="/impressum" class="hover:text-background">Impressum</a>
					<a href="/datenschutz" class="hover:text-background">Datenschutz</a>
				</div>
			</div>
		</div>

		<div class="border-t border-background/10 px-6 py-6 text-center text-xs text-background/40 sm:px-10">
			© {currentYear} Lena's Garn &amp; Blütentraum
		</div>
	</footer>
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

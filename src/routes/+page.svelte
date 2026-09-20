<script lang="ts">
	import ThreadDress from '$lib/components/svg/ThreadDress.svelte';
	import ThreadFlower from '$lib/components/svg/ThreadFlower.svelte';
	import ThreadWave from '$lib/components/svg/ThreadWave.svelte';

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

	type SynergyCard = {
		kicker: string;
		title: string;
		description: string;
		image: string;
		featured?: boolean;
	};
	const synergyCards: SynergyCard[] = [
		{
			kicker: 'Hochzeit',
			title: 'Für den wichtigsten Tag',
			description:
				'Festliche Floristik für große Hochzeitsgesellschaften – von würdevollem Kirchenschmuck bis zum Brautstrauß, der jeden Blick auf sich zieht.',
			image:
				'https://images.unsplash.com/photo-1522673607200-164d1b6ce486?auto=format&fit=crop&w=1200&q=80',
			featured: true
		},
		{
			kicker: 'Atelier',
			title: 'Maßgeschneiderte Eleganz',
			description:
				'Individuelle, würdevolle Schnitte für Kleider und Röcke, die perfekt sitzen und Ihre Werte widerspiegeln.',
			image:
				'https://images.unsplash.com/photo-1618244972963-dbee1a7edc95?auto=format&fit=crop&w=1200&q=80'
		},
		{
			kicker: 'Harmonie',
			title: 'Abgestimmte Harmonie',
			description:
				'Wenn der Brautstrauß exakt zum Garn des maßgeschneiderten Kleides passt – aus einer Hand gedacht, bis ins letzte Detail.',
			image:
				'https://images.unsplash.com/photo-1606800052052-a08af7148866?auto=format&fit=crop&w=1200&q=80'
		}
	];

	// Vorher-Nachher-Slider: edles Garn/Stoff (Vision) zu einem fließenden,
	// maßgeschneiderten Stück (Unikat).
	let sliderValue = $state(50);
	const sliderBeforeImage =
		'https://images.unsplash.com/photo-1670764732222-e787bccd934f?auto=format&fit=crop&w=1200&q=80';
	const sliderAfterImage =
		'https://images.unsplash.com/photo-1758186168047-00dd2621d27f?auto=format&fit=crop&w=1200&q=80';

	// Interaktiver 3-Schritte-Inspirations-Finder.
	const finderServices = [
		'Brautfloristik',
		'Maßanfertigung Rock/Kleid',
		'Event-Dekoration',
		'Änderungen'
	] as const;
	const finderStyles = ['Klassisch & Zeitlos', 'Zart & Romantisch', 'Festlich & Elegant'] as const;
	let finderStep = $state(1);
	let finderService: (typeof finderServices)[number] | '' = $state('');
	let finderStyle: (typeof finderStyles)[number] | '' = $state('');
	function chooseFinderService(service: (typeof finderServices)[number]) {
		finderService = service;
		finderStep = 2;
	}
	function chooseFinderStyle(style: (typeof finderStyles)[number]) {
		finderStyle = style;
		finderStep = 3;
	}
	function restartFinder() {
		finderStep = 1;
		finderService = '';
		finderStyle = '';
	}

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
	const finderWhatsappHref = $derived(
		`https://wa.me/4915146159350?text=${encodeURIComponent(`Hallo Lena, ich habe mit dem Inspirations-Finder geschaut: ${finderService}, Stil: ${finderStyle}. Lass uns das besprechen!`)}`
	);
	const finderEmailHref = $derived(
		`mailto:${email}?subject=${encodeURIComponent('Inspirations-Anfrage')}&body=${encodeURIComponent(`Ich interessiere mich für: ${finderService}\nMein Wunschstil: ${finderStyle}`)}`
	);
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
	let heroThreadPathEl: SVGPathElement;
	let aboutFramePathEl: SVGPathElement;
	let heroNeedleEl: SVGGElement;
	let frameNeedleEl: SVGGElement;
	let ctaNeedleEl: SVGGElement;
	let curtainEl: HTMLDivElement;
	let scrollHintEl: HTMLDivElement;
	let scrollLineEl: HTMLSpanElement;
	let splitEl: HTMLElement;
	let splitHeadingEl: HTMLDivElement;
	let mobileCards: HTMLAnchorElement[] = $state([]);
	let desktopPanels: HTMLAnchorElement[] = $state([]);
	let bentoEl: HTMLElement;
	let sliderSectionEl: HTMLElement;
	let finderSectionEl: HTMLElement;
	let lookbookEl: HTMLElement;

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

				// Die Nadel lebt als <g> direkt im selben SVG wie ihr Faden-Pfad,
				// darum reicht eine reine Lokalkoordinaten-Transformation – kein
				// Viewport-Umrechnen, also kein Versatz. Nur eine Nadel ist je
				// Zeitpunkt sichtbar: sobald ein anderer Pfad übernimmt, wird die
				// vorige ausgeblendet.
				// eslint-disable-next-line @typescript-eslint/no-explicit-any
				let activeNeedleEl: SVGGElement | null = null;
				const moveNeedle = (
					pathEl: SVGPathElement,
					progress: number,
					needleGroupEl?: SVGGElement | null
				) => {
					if (reduceMotion || !needleGroupEl) return;
					const pathLength = pathEl.getTotalLength();
					const currentLen = pathLength * progress;
					if (currentLen <= 0.5) {
						if (needleGroupEl === activeNeedleEl) {
							gsap.set(needleGroupEl, { opacity: 0 });
							activeNeedleEl = null;
						}
						return;
					}
					const p = pathEl.getPointAtLength(currentLen);
					const pNext = pathEl.getPointAtLength(Math.min(currentLen + 2, pathLength));
					const angle = Math.atan2(pNext.y - p.y, pNext.x - p.x) * (180 / Math.PI);
					if (activeNeedleEl && activeNeedleEl !== needleGroupEl) {
						gsap.set(activeNeedleEl, { opacity: 0 });
					}
					activeNeedleEl = needleGroupEl;
					gsap.set(needleGroupEl, { opacity: 1, x: p.x, y: p.y, rotation: angle });
				};

				gsap.set(curtainEl, { yPercent: 100 });
				gsap.set(aboutImageWrapEl, { opacity: 0, scale: 0.95, y: 28 });
				gsap.set(aboutTextEl, { opacity: 0, y: 20 });

				const heroThreadLen = heroThreadPathEl.getTotalLength();
				gsap.set(heroThreadPathEl, {
					strokeDasharray: heroThreadLen,
					strokeDashoffset: heroThreadLen,
					opacity: 0
				});
				const aboutFrameLen = aboutFramePathEl.getTotalLength();
				gsap.set(aboutFramePathEl, { strokeDasharray: aboutFrameLen, strokeDashoffset: aboutFrameLen });

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

					// Pin-Distanz deutlich verlängert (vorher +=40%): die Animation
					// läuft jetzt über eine viel längere Scroll-Strecke und wirkt
					// dadurch spürbar sanfter und eleganter.
					//
					// Wichtig gegen toten Scroll-Raum: GSAP reserviert für ein
					// gepinntes Element standardmäßig einen Spacer in Höhe von
					// (Elementhöhe + Pin-Distanz). Wäre heroEl selbst 100svh hoch,
					// müsste man nach dem Lösen des Pins zusätzlich noch eine volle
					// Bildschirmhöhe "leer" weiterscrollen, bevor "Zwei Welten"
					// erscheint. Deshalb ist heroEl selbst nur noch ein winziges
					// (h-px) Trigger-Element; die eigentliche, 100svh hohe visuelle
					// Szene lebt in einem absolut positionierten Innen-Wrapper
					// (heroSceneEl) direkt darunter. So bleibt der Spacer nur so groß
					// wie die Pin-Distanz selbst, und "Zwei Welten" schließt exakt in
					// dem Moment an, in dem sich das Pin löst.
					//
					// Sobald "Zwei Welten" in den letzten Scroll-Pixeln des Pins in
					// den Viewport hineinragt, würde es den noch aktiv gepinnten Hero
					// optisch überdecken (spätere DOM-Position gewinnt sonst gegen ein
					// fixiertes, aber z-index:auto Element). Der z-index wird daher
					// nicht auf heroEl selbst gesetzt – GSAP überschreibt dessen Inline-
					// Styles laufend bei jedem Pin-Update –, sondern auf den von GSAP
					// erzeugten Pin-Spacer-Wrapper, den GSAP nicht fortlaufend
					// neu beschreibt. Nur während das Pin aktiv ist, liegt er oben;
					// danach fällt er sofort zurück, damit "Zwei Welten" ohne
					// Verzögerung sichtbar wird.
					const flyTl = gsap.timeline({
						scrollTrigger: {
							trigger: heroEl,
							start: 'top top',
							end: '+=300%',
							pin: true,
							scrub: 1,
							anticipatePin: 1,
							// onUpdate statt onToggle: ScrollTrigger.refresh()-Läufe
							// (z. B. durch spät ladende Bilder weiter unten auf der
							// Seite) überschreiben den z-index sonst wieder mit "auto".
							// onUpdate feuert bei jedem Scroll-Tick erneut und stellt
							// den korrekten Wert so laufend selbst wieder her.
							onUpdate: (self) => {
								gsap.set(heroEl.parentElement, { zIndex: self.isActive ? 20 : 0 });
							},
							onRefresh: (self) => {
								gsap.set(heroEl.parentElement, { zIndex: self.isActive ? 20 : 0 });
							}
						},
						onStart: () => {
							glowPulse?.kill();
							scrollLinePulse?.kill();
						}
					});

					flyTl
						// Phase 1 (0% – 45%): Logo + Glow fliegen cineastisch weg.
						// Der Faden bleibt bis ~20% Scroll-Fortschritt unsichtbar und
						// löst sich erst dann sichtbar vom Logo, um nach unten zu wachsen.
						.to(scrollHintEl, { opacity: 0, y: 8, duration: 0.05, ease: 'none' }, 0)
						.fromTo(
							heroThreadPathEl,
							{ opacity: 0 },
							{ opacity: 1, ease: 'none', duration: 0.05 },
							0.2
						)
						.to(heroThreadPathEl, {
							strokeDashoffset: 0,
							ease: 'none',
							duration: 0.35,
							onUpdate: function () {
								moveNeedle(heroThreadPathEl, this.progress(), heroNeedleEl);
							}
						}, 0.2)
						// Der kurze Hero-Faden bleibt sonst als fixer, horizontal
						// zentrierter Strich stehen und würde Lenas Foto (das auf Mobile
						// ebenfalls zentriert ist) durchschneiden. Er blendet aus, sobald
						// der Rahmen-Faden um das Bild herum die Führung übernimmt.
						.to(
							heroThreadPathEl,
							{ opacity: 0, ease: 'none', duration: 0.08 },
							0.28
						)
						.to(
							logoEl,
							{
								scale: isMobile ? 2.2 : 2.6,
								yPercent: -130,
								opacity: 0,
								ease: 'power1.in',
								duration: 0.42
							},
							0.03
						)
						.to(
							glowEl,
							{
								scale: isMobile ? 2.4 : 3.2,
								opacity: 0,
								ease: 'none',
								duration: 0.45
							},
							0
						)
						// Phase 2 (30% – 75%): "Über Lena"-Szene blendet synchron ein.
						.fromTo(
							aboutImageWrapEl,
							{ opacity: 0, scale: 0.95, y: 28 },
							{ opacity: 1, scale: 1, y: 0, ease: 'power2.out', duration: 0.45 },
							0.3
						)
						.fromTo(
							aboutTextEl,
							{ opacity: 0, y: 20 },
							{ opacity: 1, y: 0, ease: 'power2.out', duration: 0.42 },
							0.33
						)
						.to(aboutFramePathEl, {
							strokeDashoffset: 0,
							ease: 'none',
							duration: 0.38,
							onUpdate: function () {
								moveNeedle(aboutFramePathEl, this.progress(), frameNeedleEl);
							}
						}, 0.3)
						// Lena bleibt bis 68% ausführlich lesbar im Bild (der lange
						// Rahmen-Zeichenvorgang ist selbst die Lesepause).
						// Phase 3 (68% – 100%): Split-Section dockt an, Lena-Szene
						// gleitet raus und der Vorhang fährt hoch – bewusst über den
						// vollen restlichen Timeline-Bereich gestreckt, damit exakt bei
						// Progress 1 (= Ende der Pin-Distanz) alles fertig ist und
						// nichts einfriert, bevor "Zwei Welten" anschließt.
						.to(
							aboutEl,
							{ opacity: 0, y: -50, ease: 'power1.in', duration: 0.3 },
							0.68
						)
						.to(
							curtainEl,
							{ yPercent: 0, ease: 'power2.inOut', duration: 0.3 },
							0.68
						)
						.to(
							headerEl,
							{
								opacity: 1,
								duration: 0.08,
								ease: 'power1.out',
								onStart: () => (headerVisible = true),
								onReverseComplete: () => (headerVisible = false)
							},
							0.9
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

					// Bento-Karten: sanftes Einblenden plus ein zart gegenläufiger
					// Parallax auf dem Bild-Wrapper (nicht auf dem Bild selbst, damit
					// der Hover-Zoom-Transform des Bildes unangetastet bleibt).
					if (bentoEl) {
						bentoEl.querySelectorAll<HTMLElement>('.bento-card').forEach((card, i) => {
							gsap.from(card, {
								opacity: 0,
								y: isMobile ? 20 : 28,
								duration: isMobile ? 0.6 : 0.8,
								delay: i * 0.1,
								ease: 'power2.out',
								scrollTrigger: { trigger: card, start: 'top 90%' }
							});
							if (!reduceMotion) {
								const parallaxEl = card.querySelector<HTMLElement>('.bento-parallax');
								if (parallaxEl) {
									gsap.fromTo(
										parallaxEl,
										{ yPercent: -3 },
										{
											yPercent: 3,
											ease: 'none',
											scrollTrigger: {
												trigger: card,
												start: 'top bottom',
												end: 'bottom top',
												scrub: true
											}
										}
									);
								}
							}
						});
					}

					// Gemeinsamer Helfer: zeichnet einen Pfad nach, sobald sein Trigger
					// von unten ins Bild kommt – so ist immer ein Stück Faden im
					// Sichtfeld, ohne dass er je "abreißt".
					const wireDraw = (
						pathEl: SVGPathElement | null,
						triggerEl: Element | null,
						start = 'top bottom',
						end = 'top 15%',
						dotEl?: SVGCircleElement | null,
						needleGroupEl?: SVGGElement | null
					) => {
						if (!pathEl || !triggerEl) return;
						const len = pathEl.getTotalLength();
						gsap.set(pathEl, { strokeDasharray: len, strokeDashoffset: len });
						if (dotEl) gsap.set(dotEl, { opacity: 0, scale: 0, transformOrigin: '50% 50%' });

						const tl = gsap.timeline({
							scrollTrigger: { trigger: triggerEl, start, end, scrub: 1.5 }
						});
						tl.to(
							pathEl,
							{
								strokeDashoffset: 0,
								ease: 'none',
								duration: 1,
								onUpdate: function () {
									moveNeedle(pathEl, this.progress(), needleGroupEl);
								}
							},
							0
						);
						if (dotEl) {
							// Der Glanzpunkt leuchtet erst auf, sobald die Linie fast fertig gezeichnet ist.
							tl.to(dotEl, { opacity: 1, scale: 1, ease: 'power1.out', duration: 0.25 }, 0.8);
						}
					};

					// Faden zeichnet im jeweiligen Kartenbereich das Kleid bzw. die Blüte,
					// mit leuchtendem Glanzpunkt an der fertig gezeichneten Spitze und der
					// Nadel, die exakt an der Zeichenspitze führt.
					const prefix = isMobile ? 'mobile' : 'desktop';
					worlds.forEach((world, i) => {
						const pathEl = document.getElementById(
							`${prefix}-thread-${world.kicker}`
						) as SVGPathElement | null;
						const dotEl = document.getElementById(
							`${prefix}-dot-${world.kicker}`
						) as SVGCircleElement | null;
						const needleGroupEl = document.getElementById(
							`${prefix}-needle-${world.kicker}`
						) as SVGGElement | null;
						wireDraw(pathEl, revealTargets[i], undefined, undefined, dotEl, needleGroupEl);
					});

					// Verbindende Wellenlinien zwischen den Stationen, damit der Faden
					// permanent im Viewport präsent bleibt.
					wireDraw(
						document.getElementById('wave-synergy') as SVGPathElement | null,
						bentoEl,
						'top bottom',
						'bottom 30%',
						null,
						document.getElementById('wave-needle-synergy') as SVGGElement | null
					);
					wireDraw(
						document.getElementById('wave-slider') as SVGPathElement | null,
						sliderSectionEl,
						'top bottom',
						'bottom 30%',
						null,
						document.getElementById('wave-needle-slider') as SVGGElement | null
					);
					wireDraw(
						document.getElementById('wave-finder') as SVGPathElement | null,
						finderSectionEl,
						'top bottom',
						'bottom 30%',
						null,
						document.getElementById('wave-needle-finder') as SVGGElement | null
					);
					wireDraw(
						document.getElementById('wave-lookbook') as SVGPathElement | null,
						lookbookEl,
						'top bottom',
						'bottom 20%',
						null,
						document.getElementById('wave-needle-lookbook') as SVGGElement | null
					);

					// Faden mündet in eine kleine Schleife über dem Anfrage-Bereich,
					// die sich am Ende zu einem Glanzpunkt schließt.
					wireDraw(
						document.getElementById('cta-loop-path') as SVGPathElement | null,
						document.getElementById('cta-loop-path'),
						undefined,
						undefined,
						document.getElementById('cta-loop-dot') as SVGCircleElement | null,
						ctaNeedleEl
					);
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
	<img src="/logo.jpg" alt="" class="h-8 w-auto mix-blend-multiply" />
	<span class="font-serif text-sm tracking-wide text-ink sm:text-base">Lena's Garn &amp; Blütentraum</span>
</header>

<main class="bg-mist">
	<section bind:this={heroEl} id="hero" class="relative h-px w-full">
	<div
		class="absolute inset-x-0 top-0 flex h-svh min-h-svh w-full flex-col items-center justify-center overflow-hidden bg-mist"
	>
		<div
			bind:this={glowEl}
			aria-hidden="true"
			style="will-change: transform, opacity;"
			class="pointer-events-none absolute left-1/2 top-1/2 h-[60vmax] w-[60vmax] -translate-x-1/2 -translate-y-1/2 rounded-full bg-accent/40 opacity-50 blur-3xl"
		></div>

		<svg
			viewBox="0 0 40 100"
			aria-hidden="true"
			style="filter: drop-shadow(0 0 6px rgba(255, 255, 255, 0.9)) drop-shadow(0 2px 8px rgba(0, 0, 0, 0.35));"
			class="pointer-events-none absolute left-1/2 top-[30%] z-20 h-[24vh] w-16 -translate-x-1/2 text-thread"
		>
			<path
				bind:this={heroThreadPathEl}
				d="M20,0 C10,20 30,40 20,60 C12,74 26,88 20,100"
				fill="none"
				stroke="currentColor"
				stroke-width="4"
				vector-effect="non-scaling-stroke"
				stroke-linecap="round"
			/>
			<defs>
				<linearGradient id="hero-needle-grad" x1="0%" y1="0%" x2="100%" y2="100%">
					<stop offset="0%" stop-color="#E2E8F0" />
					<stop offset="35%" stop-color="#FFFFFF" />
					<stop offset="70%" stop-color="#94A3B8" />
					<stop offset="100%" stop-color="#475569" />
				</linearGradient>
			</defs>
			<g
				bind:this={heroNeedleEl}
				opacity="0"
				style="filter: drop-shadow(0 2px 5px rgba(0, 0, 0, 0.5)) drop-shadow(0 0 8px rgba(255, 255, 255, 0.8));"
			>
				<g transform="scale(0.6)">
					<path d="M8,0 L17,-1.6 L44,-0.8 L52,0 L44,0.8 L17,1.6 Z" fill="url(#hero-needle-grad)" />
					<ellipse cx="6" cy="0" rx="5" ry="2.6" fill="none" stroke="url(#hero-needle-grad)" stroke-width="1.8" />
				</g>
			</g>
		</svg>

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

				<svg
					viewBox="0 0 100 120"
					preserveAspectRatio="none"
					aria-hidden="true"
					style="filter: drop-shadow(0 0 6px rgba(255, 255, 255, 0.9)) drop-shadow(0 2px 8px rgba(0, 0, 0, 0.35));"
					class="pointer-events-none absolute -inset-5 z-30 h-[calc(100%+2.5rem)] w-[calc(100%+2.5rem)] text-thread"
				>
					<path
						bind:this={aboutFramePathEl}
						d="M10,4 C-6,18 -6,58 2,94 C8,110 26,119 50,119 C74,119 92,110 98,94 C106,58 106,18 90,4"
						fill="none"
						stroke="currentColor"
						stroke-width="4"
						vector-effect="non-scaling-stroke"
						stroke-linecap="round"
					/>
					<defs>
						<linearGradient id="frame-needle-grad" x1="0%" y1="0%" x2="100%" y2="100%">
							<stop offset="0%" stop-color="#E2E8F0" />
							<stop offset="35%" stop-color="#FFFFFF" />
							<stop offset="70%" stop-color="#94A3B8" />
							<stop offset="100%" stop-color="#475569" />
						</linearGradient>
					</defs>
					<g
						bind:this={frameNeedleEl}
						opacity="0"
						style="filter: drop-shadow(0 2px 5px rgba(0, 0, 0, 0.5)) drop-shadow(0 0 8px rgba(255, 255, 255, 0.8));"
					>
						<g transform="scale(0.25)">
							<path d="M8,0 L17,-1.6 L44,-0.8 L52,0 L44,0.8 L17,1.6 Z" fill="url(#frame-needle-grad)" />
							<ellipse cx="6" cy="0" rx="5" ry="2.6" fill="none" stroke="url(#frame-needle-grad)" stroke-width="1.8" />
						</g>
					</g>
				</svg>
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
			class="pointer-events-none absolute inset-0 z-40 rounded-t-[3rem] bg-linear-to-b from-accent/20 via-nude/30 to-background shadow-[0_-20px_60px_-15px_rgba(28,29,31,0.25)]"
		></div>
	</div>
	</section>

	<section bind:this={splitEl} id="welten" class="relative bg-background">
		<div bind:this={splitHeadingEl} class="mx-auto max-w-2xl px-6 pb-10 pt-6 text-center sm:pt-10">
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
					{#if world.kicker === 'Atelier'}
						<ThreadDress
							pathId="mobile-thread-Atelier"
							dotId="mobile-dot-Atelier"
							needleId="mobile-needle-Atelier"
							needleScale={1.7}
							class="pointer-events-none absolute right-2 top-2 z-20 h-44 w-28 text-thread"
						/>
					{:else}
						<ThreadFlower
							pathId="mobile-thread-Floristik"
							dotId="mobile-dot-Floristik"
							needleId="mobile-needle-Floristik"
							needleScale={1.7}
							class="pointer-events-none absolute right-2 top-2 z-20 h-44 w-28 text-champagne"
						/>
					{/if}
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
					{#if world.kicker === 'Atelier'}
						<ThreadDress
							pathId="desktop-thread-Atelier"
							dotId="desktop-dot-Atelier"
							needleId="desktop-needle-Atelier"
							needleScale={1.1}
							class="pointer-events-none absolute right-6 top-6 z-20 h-64 w-44 text-thread lg:h-72 lg:w-48"
						/>
					{:else}
						<ThreadFlower
							pathId="desktop-thread-Floristik"
							dotId="desktop-dot-Floristik"
							needleId="desktop-needle-Floristik"
							needleScale={1.1}
							class="pointer-events-none absolute right-6 top-6 z-20 h-64 w-44 text-champagne lg:h-72 lg:w-48"
						/>
					{/if}
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

	<section bind:this={bentoEl} class="relative overflow-hidden bg-background px-6 py-20 sm:px-10 sm:py-28">
		<ThreadWave
			pathId="wave-synergy"
			needleId="wave-needle-synergy"
			needleScale={0.65}
			class="pointer-events-none absolute inset-y-0 left-2 z-10 w-10 text-thread sm:left-6 sm:w-14"
		/>

		<div class="mx-auto max-w-5xl text-center">
			<span class="text-xs uppercase tracking-[0.35em] text-accent">Zwei Handwerke für den großen Tag</span>
			<h2 class="mt-3 font-serif text-3xl text-ink sm:text-4xl">Wo Floristik und Schneiderei eins werden</h2>
		</div>

		<div class="mx-auto mt-14 grid max-w-6xl gap-5 sm:grid-cols-2 sm:gap-6">
			{#each synergyCards as card}
				<div
					class="bento-card group relative overflow-hidden rounded-[2rem] shadow-lg {card.featured
						? 'aspect-[4/5] sm:col-span-2 sm:aspect-[21/9]'
						: 'aspect-[4/5]'}"
				>
					<div class="bento-parallax absolute inset-x-0 -top-[6%] h-[112%] w-full overflow-hidden">
						<img
							src={card.image}
							alt=""
							loading="lazy"
							class="h-full w-full object-cover object-top transition-transform duration-700 ease-out group-hover:scale-110"
						/>
					</div>
					<div class="absolute inset-0 bg-linear-to-t from-ink/75 via-ink/15 to-transparent"></div>
					<div class="relative flex h-full flex-col justify-end gap-2 p-7 text-background sm:p-9">
						<span class="text-xs uppercase tracking-[0.3em] text-background/80">{card.kicker}</span>
						<h3 class="font-serif text-2xl sm:text-3xl">{card.title}</h3>
						<p class="max-w-md text-sm leading-relaxed text-background/85 sm:text-base">
							{card.description}
						</p>
					</div>
				</div>
			{/each}
		</div>
	</section>

	<section
		bind:this={sliderSectionEl}
		class="relative overflow-hidden bg-nude/15 px-6 py-20 sm:px-10 sm:py-28"
	>
		<ThreadWave
			pathId="wave-slider"
			needleId="wave-needle-slider"
			needleScale={0.65}
			class="pointer-events-none absolute inset-y-0 right-2 z-10 w-10 text-champagne sm:right-6 sm:w-14"
		/>

		<div class="mx-auto max-w-2xl text-center">
			<span class="text-xs uppercase tracking-[0.35em] text-accent">Handwerk fühlbar machen</span>
			<h2 class="mt-3 font-serif text-3xl text-ink sm:text-4xl">Von der Vision zum Unikat</h2>
			<p class="mt-3 text-sm text-ink/70 sm:text-base">
				Bewegen Sie den Regler und erleben Sie, wie aus feinem Garn ein maßgeschneidertes Unikat
				entsteht.
			</p>
		</div>

		<div
			class="relative mx-auto mt-12 aspect-[3/4] w-full max-w-md touch-none overflow-hidden rounded-[2.5rem] shadow-[0_40px_80px_-30px_rgba(28,29,31,0.35)]"
		>
			<img
				src={sliderAfterImage}
				alt="Das fertige Unikat: ein maßgeschneiderter Rock"
				class="absolute inset-0 h-full w-full object-cover object-top"
			/>
			<div
				class="absolute inset-0 h-full w-full overflow-hidden"
				style="clip-path: inset(0 {100 - sliderValue}% 0 0);"
			>
				<img
					src={sliderBeforeImage}
					alt="Feines Garn als Ausgangspunkt"
					class="absolute inset-0 h-full w-full object-cover object-top"
				/>
			</div>

			<div
				aria-hidden="true"
				class="pointer-events-none absolute inset-y-0 z-10 w-[3px] -translate-x-1/2 bg-linear-to-b from-champagne/0 via-champagne to-champagne/0 shadow-[0_0_14px_rgba(212,175,55,0.75)]"
				style="left: {sliderValue}%;"
			></div>
			<div
				aria-hidden="true"
				class="pointer-events-none absolute top-1/2 z-10 flex h-11 w-11 -translate-x-1/2 -translate-y-1/2 items-center justify-center rounded-full border-2 border-champagne bg-background text-champagne shadow-lg"
				style="left: {sliderValue}%;"
			>
				<span class="text-sm">↔</span>
			</div>

			<span
				class="pointer-events-none absolute left-4 top-4 rounded-full bg-background/85 px-3 py-1 text-xs font-medium uppercase tracking-[0.2em] text-ink backdrop-blur-sm"
			>
				Die Vision
			</span>
			<span
				class="pointer-events-none absolute right-4 top-4 rounded-full bg-ink/70 px-3 py-1 text-xs font-medium uppercase tracking-[0.2em] text-background backdrop-blur-sm"
			>
				Das Unikat
			</span>

			<input
				type="range"
				min="0"
				max="100"
				bind:value={sliderValue}
				aria-label="Vorher-Nachher-Vergleich: Garn zu maßgeschneidertem Unikat"
				class="absolute inset-0 z-20 h-full w-full cursor-ew-resize appearance-none bg-transparent opacity-0"
			/>
		</div>
	</section>

	<section
		bind:this={finderSectionEl}
		class="relative overflow-hidden bg-background px-6 py-20 sm:px-10 sm:py-28"
	>
		<ThreadWave
			pathId="wave-finder"
			needleId="wave-needle-finder"
			needleScale={0.65}
			class="pointer-events-none absolute inset-y-0 left-2 z-10 w-10 text-thread sm:left-6 sm:w-14"
		/>

		<div class="mx-auto max-w-2xl text-center">
			<span class="text-xs uppercase tracking-[0.35em] text-accent">Ihr persönlicher Inspirations-Finder</span>
			<h2 class="mt-3 font-serif text-3xl text-ink sm:text-4xl">Finden Sie Ihren Stil</h2>
		</div>

		<div class="mx-auto mt-10 flex items-center justify-center gap-3">
			{#each [1, 2, 3] as step}
				<span
					class="flex h-8 w-8 items-center justify-center rounded-full border font-serif text-sm transition-colors {finderStep >=
					step
						? 'border-accent bg-accent text-background'
						: 'border-ink/20 text-ink/50'}"
				>
					{step}
				</span>
				{#if step < 3}
					<span class="h-px w-8 {finderStep > step ? 'bg-accent' : 'bg-ink/15'}"></span>
				{/if}
			{/each}
		</div>

		<div
			class="mx-auto mt-10 max-w-xl rounded-[2.5rem] border border-ink/10 bg-nude/10 p-8 text-center shadow-[0_30px_70px_-40px_rgba(28,29,31,0.3)] sm:p-12"
		>
			{#if finderStep === 1}
				<h3 class="font-serif text-xl text-ink">Was dürfen wir für dich kreieren?</h3>
				<div class="mt-6 flex flex-wrap justify-center gap-3">
					{#each finderServices as service}
						<button
							type="button"
							onclick={() => chooseFinderService(service)}
							class="rounded-full border border-ink/15 px-5 py-2.5 text-sm font-medium text-ink transition-colors hover:border-accent hover:bg-accent hover:text-background"
						>
							{service}
						</button>
					{/each}
				</div>
			{:else if finderStep === 2}
				<h3 class="font-serif text-xl text-ink">Dein Wunschstil</h3>
				<div class="mt-6 flex flex-wrap justify-center gap-3">
					{#each finderStyles as style}
						<button
							type="button"
							onclick={() => chooseFinderStyle(style)}
							class="rounded-full border border-ink/15 px-5 py-2.5 text-sm font-medium text-ink transition-colors hover:border-accent hover:bg-accent hover:text-background"
						>
							{style}
						</button>
					{/each}
				</div>
				<button
					type="button"
					onclick={() => (finderStep = 1)}
					class="mt-6 text-xs uppercase tracking-[0.2em] text-ink/50 hover:text-accent"
				>
					← Zurück
				</button>
			{:else}
				<h3 class="font-serif text-xl text-ink">Ihre Inspiration ist bereit</h3>
				<p class="mt-3 text-sm text-ink/70">{finderService} · {finderStyle}</p>
				<div class="mt-8 flex flex-col items-center justify-center gap-3 sm:flex-row">
					<a
						href={finderWhatsappHref}
						target="_blank"
						rel="noopener noreferrer"
						class="inline-flex items-center gap-2 rounded-full bg-accent px-6 py-3 text-sm font-medium text-background transition-colors hover:bg-accent-light"
					>
						Jetzt besprechen
					</a>
					<a
						href={finderEmailHref}
						class="inline-flex items-center gap-2 rounded-full border border-ink/20 px-6 py-3 text-sm font-medium text-ink transition-colors hover:bg-ink hover:text-background"
					>
						Per E-Mail
					</a>
				</div>
				<button
					type="button"
					onclick={restartFinder}
					class="mt-6 text-xs uppercase tracking-[0.2em] text-ink/50 hover:text-accent"
				>
					Neu starten
				</button>
			{/if}
		</div>
	</section>

	<section bind:this={lookbookEl} class="relative overflow-hidden bg-nude/10 py-20 sm:py-28">
		<ThreadWave
			pathId="wave-lookbook"
			needleId="wave-needle-lookbook"
			needleScale={0.65}
			class="pointer-events-none absolute inset-y-0 right-2 z-10 w-10 text-champagne sm:right-6 sm:w-14"
		/>

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
		<svg
			viewBox="0 0 120 80"
			aria-hidden="true"
			style="filter: drop-shadow(0 0 6px rgba(255, 255, 255, 0.9)) drop-shadow(0 2px 8px rgba(0, 0, 0, 0.35));"
			class="pointer-events-none relative z-10 mx-auto mb-2 block h-20 w-32 text-champagne"
		>
			<path
				id="cta-loop-path"
				d="M10,40 C10,20 30,10 45,20 C55,27 55,40 45,45 C35,50 30,38 38,32 C46,26 65,26 75,35 C85,44 85,60 70,65 C58,69 50,58 60,52"
				fill="none"
				stroke="currentColor"
				stroke-width="4"
				vector-effect="non-scaling-stroke"
				stroke-linecap="round"
				stroke-linejoin="round"
			/>
			<circle id="cta-loop-dot" cx="60" cy="52" r="4" fill="currentColor" opacity="0" />
			<defs>
				<linearGradient id="cta-needle-grad" x1="0%" y1="0%" x2="100%" y2="100%">
					<stop offset="0%" stop-color="#E2E8F0" />
					<stop offset="35%" stop-color="#FFFFFF" />
					<stop offset="70%" stop-color="#94A3B8" />
					<stop offset="100%" stop-color="#475569" />
				</linearGradient>
			</defs>
			<g
				bind:this={ctaNeedleEl}
				opacity="0"
				style="filter: drop-shadow(0 2px 5px rgba(0, 0, 0, 0.5)) drop-shadow(0 0 8px rgba(255, 255, 255, 0.8));"
			>
				<g transform="scale(1)">
					<path d="M8,0 L17,-1.6 L44,-0.8 L52,0 L44,0.8 L17,1.6 Z" fill="url(#cta-needle-grad)" />
					<ellipse cx="6" cy="0" rx="5" ry="2.6" fill="none" stroke="url(#cta-needle-grad)" stroke-width="1.8" />
				</g>
			</g>
		</svg>

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

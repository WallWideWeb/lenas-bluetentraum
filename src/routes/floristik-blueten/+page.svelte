<script lang="ts">
	const services = [
		{
			title: 'Hochzeitsfloristik',
			description:
				'Brautsträuße, Tischdekorationen und florale Konzepte, die perfekt auf Ihren großen Tag abgestimmt sind.',
			featured: true
		},
		{
			title: 'Eventdekoration',
			description: 'Stimmungsvolle Blumenarrangements für Feiern und besondere Anlässe.'
		},
		{
			title: 'Trockenblumen-Kompositionen',
			description: 'Zeitlose Sträuße und Kränze, die weit über den Moment hinaus Freude bereiten.'
		},
		{
			title: 'Saisonale Sträuße',
			description: 'Wechselnde Kompositionen aus dem, was die Saison gerade an Schönstem zu bieten hat.'
		}
	];
</script>

<svelte:head>
	<title>Floristik &amp; Blüten – Lena's Garn &amp; Blütentraum</title>
	<meta
		name="description"
		content="Hochzeitsfloristik, Eventdekoration und Trockenblumen-Kompositionen von Lena's Garn & Blütentraum."
	/>
</svelte:head>

<div class="min-h-svh bg-background">
	<header
		class="sticky top-0 z-50 flex items-center justify-between gap-4 border-b border-ink/10 bg-background/80 px-6 py-4 backdrop-blur-md sm:px-10"
	>
		<a href="/" class="flex items-center gap-3">
			<img src="/logo.jpg" alt="" class="h-8 w-auto" />
			<span class="font-serif text-sm tracking-wide text-ink sm:text-base"
				>Lena's Garn &amp; Blütentraum</span
			>
		</a>
		<a
			href="/"
			class="inline-flex items-center gap-2 rounded-full border border-ink/15 px-4 py-2 text-sm font-medium text-ink/70 transition-colors hover:border-accent/40 hover:text-ink"
		>
			<span aria-hidden="true">&larr;</span> Zurück zur Übersicht
		</a>
	</header>

	<section class="relative h-[55vh] min-h-[380px] w-full overflow-hidden sm:h-[65vh]">
		<img
			src="https://images.unsplash.com/photo-1487530811176-3780de880c2d?auto=format&fit=crop&w=1600&q=80"
			alt=""
			class="absolute inset-0 h-full w-full object-cover"
		/>
		<div class="absolute inset-0 bg-linear-to-t from-ink/70 via-ink/20 to-transparent"></div>
		<div class="relative flex h-full flex-col items-start justify-end gap-3 px-6 pb-10 sm:px-10 sm:pb-14">
			<span class="text-xs uppercase tracking-[0.35em] text-background/80">Floristik</span>
			<h1 class="font-serif text-4xl text-background sm:text-5xl">Floristik &amp; Blüten</h1>
		</div>
	</section>

	<section class="mx-auto max-w-5xl px-6 py-14 sm:px-10 sm:py-20">
		<p class="max-w-2xl text-lg leading-relaxed text-ink/80">
			Von der duftenden Hochzeitsfloristik bis zur zeitlosen Trockenblumen-Komposition – Lena
			verbindet frische und getrocknete Blüten zu Arrangements, die jeden Anlass unverwechselbar
			machen. Jede Komposition wird individuell abgestimmt, mit einem feinen Gespür für Farbe, Form
			und Saison.
		</p>

		<div class="mt-14 grid grid-cols-1 gap-5 sm:grid-cols-2 lg:grid-cols-4">
			{#each services as service}
				<div
					class="rounded-3xl border border-ink/10 p-7 {service.featured
						? 'bg-rose/20 sm:col-span-2 lg:col-span-2 lg:row-span-2'
						: 'bg-background'} {!service.featured && service.title === 'Saisonale Sträuße'
						? 'sm:col-span-2 lg:col-span-2'
						: ''}"
				>
					{#if service.featured}
						<span class="text-xs uppercase tracking-[0.3em] text-accent">Kernleistung</span>
					{/if}
					<h3 class="mt-2 font-serif text-xl text-ink {service.featured ? 'text-2xl' : ''}">
						{service.title}
					</h3>
					<p class="mt-3 text-sm leading-relaxed text-ink/70">{service.description}</p>
				</div>
			{/each}
		</div>
	</section>

	<section class="bg-rose/10 px-6 py-16 sm:px-10 sm:py-24">
		<div class="mx-auto grid max-w-5xl items-center gap-10 sm:grid-cols-2">
			<div>
				<span class="text-xs uppercase tracking-[0.3em] text-accent">Material-Philosophie</span>
				<h2 class="mt-3 font-serif text-2xl text-ink sm:text-3xl">Frisch, saisonal, ehrlich</h2>
				<p class="mt-4 text-sm leading-relaxed text-ink/75 sm:text-base">
					Wir setzen auf Blüten aus der Saison, ergänzt durch sorgfältig getrocknete Kompositionen,
					die auch nach dem großen Tag noch lange erfreuen. So entsteht Floristik, die zur
					Jahreszeit passt und bewusst mit der Natur statt gegen sie arbeitet.
				</p>
			</div>
			<div>
				<img
					src="https://images.unsplash.com/photo-1519378058457-4c29a0a2efac?auto=format&fit=crop&w=900&q=80"
					alt=""
					class="aspect-4/5 w-full rounded-[2rem] object-cover shadow-[0_30px_60px_-25px_rgba(28,29,31,0.3)]"
				/>
			</div>
		</div>
	</section>

	<section class="px-6 py-16 text-center sm:px-10 sm:py-20">
		<a
			href="/"
			class="inline-flex items-center gap-2 rounded-full bg-accent px-6 py-3 text-sm font-medium text-background transition-colors hover:bg-accent-light"
		>
			<span aria-hidden="true">&larr;</span> Zurück zur Übersicht
		</a>
	</section>
</div>

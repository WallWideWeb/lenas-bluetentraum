<script lang="ts">
	const services = [
		{
			title: 'Maßanfertigungen',
			description:
				'Kleidung und Accessoires, die exakt nach Ihren Maßen und Wünschen entstehen – von der ersten Skizze bis zur letzten Naht.',
			featured: true
		},
		{
			title: 'Änderungsschneiderei',
			description: 'Vom kleinen Saum bis zur kompletten Umarbeitung – mit Liebe zum Detail.'
		},
		{
			title: 'Handgefertigte Unikate',
			description: 'Gestrickte und genähte Einzelstücke aus sorgfältig ausgewählten Garnen.'
		},
		{
			title: 'Beratung & Stoffauswahl',
			description:
				'Gemeinsam finden wir Schnitt, Farbe und Material, das zu Ihnen und dem Anlass passt.'
		}
	];
</script>

<svelte:head>
	<title>Atelier &amp; Garn – Lena's Garn &amp; Blütentraum</title>
	<meta
		name="description"
		content="Maßanfertigungen, Änderungsschneiderei und handgefertigte Unikate im Atelier von Lena's Garn & Blütentraum."
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
			src="https://images.unsplash.com/photo-1556905055-8f358a7a47b2?auto=format&fit=crop&w=1600&q=80"
			alt=""
			class="absolute inset-0 h-full w-full object-cover"
		/>
		<div class="absolute inset-0 bg-linear-to-t from-ink/70 via-ink/20 to-transparent"></div>
		<div class="relative flex h-full flex-col items-start justify-end gap-3 px-6 pb-10 sm:px-10 sm:pb-14">
			<span class="text-xs uppercase tracking-[0.35em] text-background/80">Atelier</span>
			<h1 class="font-serif text-4xl text-background sm:text-5xl">Atelier &amp; Garn</h1>
		</div>
	</section>

	<section class="mx-auto max-w-5xl px-6 py-14 sm:px-10 sm:py-20">
		<p class="max-w-2xl text-lg leading-relaxed text-ink/80">
			In Lenas Atelier treffen traditionelles Schneiderhandwerk und ein Gespür für feine Garne
			aufeinander. Ob Maßanfertigung, liebevolle Änderung oder ein handgestricktes Unikat – jedes
			Stück entsteht in Ruhe, mit hochwertigen Materialien und viel Aufmerksamkeit fürs Detail.
		</p>

		<div class="mt-14 grid grid-cols-1 gap-5 sm:grid-cols-2 lg:grid-cols-4">
			{#each services as service}
				<div
					class="rounded-3xl border border-ink/10 p-7 {service.featured
						? 'bg-nude/25 sm:col-span-2 lg:col-span-2 lg:row-span-2'
						: 'bg-background'} {!service.featured && service.title === 'Beratung & Stoffauswahl'
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

	<section class="bg-nude/10 px-6 py-16 sm:px-10 sm:py-24">
		<div class="mx-auto grid max-w-5xl items-center gap-10 sm:grid-cols-2">
			<div class="order-2 sm:order-1">
				<span class="text-xs uppercase tracking-[0.3em] text-accent">Material-Philosophie</span>
				<h2 class="mt-3 font-serif text-2xl text-ink sm:text-3xl">Nur, was der Zeit stand hält</h2>
				<p class="mt-4 text-sm leading-relaxed text-ink/75 sm:text-base">
					Wir arbeiten bevorzugt mit Naturfasern wie Wolle, Leinen und Bio-Baumwolle – Materialien,
					die atmen, mit der Zeit schöner werden und lange Freude bereiten. Jedes Garn wird
					sorgfältig ausgewählt, bevor es zu einem Kleidungsstück wird, das man wirklich trägt.
				</p>
			</div>
			<div class="order-1 sm:order-2">
				<img
					src="https://images.unsplash.com/photo-1445205170230-053b83016050?auto=format&fit=crop&w=900&q=80"
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

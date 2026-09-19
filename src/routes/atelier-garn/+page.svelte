<script lang="ts">
	const features = [
		{
			title: 'Maßanfertigungen',
			description: 'Kleidung und Accessoires, exakt nach Ihren Maßen und Wünschen gefertigt.'
		},
		{
			title: 'Änderungsschneiderei',
			description: 'Vom kleinen Saum bis zur kompletten Umarbeitung – mit Liebe zum Detail.'
		},
		{
			title: 'Handgefertigte Unikate',
			description: 'Gestrickte und genähte Einzelstücke aus sorgfältig ausgewählten Garnen.'
		}
	];
</script>

<svelte:head>
	<title>Atelier &amp; Garn – Lena's Garn &amp; Blütentraum</title>
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
			class="inline-flex items-center gap-2 text-sm font-medium text-ink/70 transition-colors hover:text-ink"
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

	<section class="mx-auto max-w-3xl px-6 py-14 sm:px-10 sm:py-20">
		<p class="text-lg leading-relaxed text-ink/80">
			In Lenas Atelier treffen traditionelles Schneiderhandwerk und ein Gespür für feine Garne
			aufeinander. Ob Maßanfertigung, liebevolle Änderung oder ein handgestricktes Unikat – jedes
			Stück entsteht in Ruhe, mit hochwertigen Materialien und viel Aufmerksamkeit fürs Detail.
		</p>

		<div class="mt-12 grid gap-8 sm:grid-cols-3">
			{#each features as feature}
				<div class="rounded-2xl border border-ink/10 bg-nude/20 p-6">
					<h3 class="font-serif text-lg text-ink">{feature.title}</h3>
					<p class="mt-2 text-sm text-ink/70">{feature.description}</p>
				</div>
			{/each}
		</div>

		<div class="mt-14 flex justify-center">
			<a
				href="/"
				class="inline-flex items-center gap-2 rounded-full bg-accent px-6 py-3 text-sm font-medium text-background transition-colors hover:bg-accent-light"
			>
				<span aria-hidden="true">&larr;</span> Zurück zur Übersicht
			</a>
		</div>
	</section>
</div>

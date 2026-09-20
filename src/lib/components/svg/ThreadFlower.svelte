<script lang="ts">
	let {
		pathId,
		dotId,
		needleId,
		needleScale = 1,
		class: className = ''
	}: {
		pathId: string;
		dotId?: string;
		needleId?: string;
		needleScale?: number;
		class?: string;
	} = $props();
</script>

<!--
	Single continuous-line peony/blossom: a 5-petal mathematical rose curve
	(r = R·cos(5θ)) flowing into a stem with a small leaf flourish.
-->
<svg
	viewBox="0 0 200 300"
	class={className}
	style="filter: drop-shadow(0 0 6px rgba(255, 255, 255, 0.9)) drop-shadow(0 2px 8px rgba(0, 0, 0, 0.35));"
	aria-hidden="true"
	focusable="false"
>
	<path
		id={pathId}
		d="M178.0,78.0 L175.6,81.7 L168.5,84.7 L157.2,86.5 L142.5,86.5 L125.5,84.4 L107.3,80.2 L89.2,74.1 L72.4,66.6 L58.0,58.1 L46.8,49.6 L39.5,41.8 L36.4,35.5 L37.4,31.6 L42.3,30.6 L50.4,33.1 L61.0,39.0 L73.1,48.3 L85.6,60.5 L97.7,74.9 L108.5,90.7 L117.1,106.6 L123.3,121.6 L126.8,134.6 L127.6,144.6 L126.0,150.6 L122.5,152.3 L117.8,149.2 L112.7,141.6 L107.7,129.8 L103.6,114.6 L100.9,96.9 L100.0,78.0 L100.9,59.1 L103.6,41.4 L107.7,26.2 L112.7,14.4 L117.8,6.8 L122.5,3.7 L126.0,5.4 L127.6,11.4 L126.8,21.4 L123.3,34.4 L117.1,49.4 L108.5,65.3 L97.7,81.1 L85.6,95.5 L73.1,107.7 L61.0,117.0 L50.4,122.9 L42.3,125.4 L37.4,124.4 L36.4,120.5 L39.5,114.2 L46.8,106.4 L58.0,97.9 L72.4,89.4 L89.2,81.9 L107.3,75.8 L125.5,71.6 L142.5,69.5 L157.2,69.5 L168.5,71.3 L175.6,74.3 L178.0,78.0 C174.0,108.0 188.0,148.0 176.0,188.0 C148.0,198.0 133.0,186.0 123.0,196.0"
		fill="none"
		stroke="currentColor"
		stroke-width="4"
		vector-effect="non-scaling-stroke"
		stroke-linecap="round"
		stroke-linejoin="round"
	/>
	{#if dotId}
		<circle id={dotId} cx="123" cy="196" r="4" fill="currentColor" opacity="0" />
	{/if}
	{#if needleId}
		<defs>
			<linearGradient id="{needleId}-grad" x1="0%" y1="0%" x2="100%" y2="100%">
				<stop offset="0%" stop-color="#E2E8F0" />
				<stop offset="35%" stop-color="#FFFFFF" />
				<stop offset="70%" stop-color="#94A3B8" />
				<stop offset="100%" stop-color="#475569" />
			</linearGradient>
		</defs>
		<g
			id={needleId}
			opacity="0"
			style="filter: drop-shadow(0 2px 5px rgba(0, 0, 0, 0.5)) drop-shadow(0 0 8px rgba(255, 255, 255, 0.8));"
		>
			<g transform="scale({needleScale})">
				<path
					d="M8,0 L17,-1.6 L44,-0.8 L52,0 L44,0.8 L17,1.6 Z"
					fill="url(#{needleId}-grad)"
				/>
				<ellipse
					cx="6"
					cy="0"
					rx="5"
					ry="2.6"
					fill="none"
					stroke="url(#{needleId}-grad)"
					stroke-width="1.8"
				/>
			</g>
		</g>
	{/if}
</svg>

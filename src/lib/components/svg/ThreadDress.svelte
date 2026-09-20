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
	Single continuous-line silhouette of a gown on a hanger.
	Mirror-symmetric outline, one unbroken stroke starting/ending at the hook.
-->
<svg
	viewBox="0 0 200 320"
	class={className}
	style="filter: drop-shadow(0 0 6px rgba(255, 255, 255, 0.9)) drop-shadow(0 2px 8px rgba(0, 0, 0, 0.35)) drop-shadow(0 0 12px rgba(200, 162, 122, 0.65));"
	aria-hidden="true"
	focusable="false"
>
	<path
		id={pathId}
		d="M 100.0,6.0 C 104.0,6.0 106.0,10.0 104.0,14.0 C 108.0,18.0 112.0,20.0 115.0,22.0 Q 128.0,26.0 135.0,34.0 C 140.0,42.0 145.0,50.0 142.0,60.0 C 138.0,80.0 128.0,105.0 125.0,130.0 C 130.0,150.0 140.0,170.0 145.0,190.0 C 155.0,230.0 168.0,265.0 175.0,300.0 Q 140.0,314.0 100.0,312.0 Q 60.0,314.0 25.0,300.0 C 32.0,265.0 45.0,230.0 55.0,190.0 C 60.0,170.0 70.0,150.0 75.0,130.0 C 72.0,105.0 62.0,80.0 58.0,60.0 C 55.0,50.0 60.0,42.0 65.0,34.0 Q 72.0,26.0 85.0,22.0 C 88.0,20.0 92.0,18.0 96.0,14.0 C 94.0,10.0 96.0,6.0 100.0,6.0"
		fill="none"
		stroke="currentColor"
		stroke-width="4"
		vector-effect="non-scaling-stroke"
		stroke-linecap="round"
		stroke-linejoin="round"
	/>
	{#if dotId}
		<circle id={dotId} cx="100" cy="6" r="4" fill="currentColor" opacity="0" />
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
			style="filter: drop-shadow(0 2px 5px rgba(0, 0, 0, 0.5)) drop-shadow(0 0 8px rgba(255, 255, 255, 0.8)) drop-shadow(0 0 6px rgba(212, 175, 55, 0.85));"
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
				<!-- Funkelnder Partikelschweif, der der Nadel beim Nähen hinterherzieht -->
				<circle cx="-9" cy="1.6" r="1.7" fill="#F7E7C4" opacity="0.85">
					<animate attributeName="opacity" values="0.85;0.25;0.85" dur="0.9s" repeatCount="indefinite" />
				</circle>
				<circle cx="-21" cy="-1.4" r="1.2" fill="#F7E7C4" opacity="0.55">
					<animate attributeName="opacity" values="0.55;0.15;0.55" dur="1.3s" repeatCount="indefinite" />
				</circle>
				<circle cx="-33" cy="1" r="0.8" fill="#F7E7C4" opacity="0.3">
					<animate attributeName="opacity" values="0.3;0.05;0.3" dur="1.6s" repeatCount="indefinite" />
				</circle>
			</g>
		</g>
	{/if}
</svg>

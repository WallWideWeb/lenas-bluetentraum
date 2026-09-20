<script lang="ts">
	let {
		pathId,
		needleId,
		needleScale = 1,
		class: className = ''
	}: {
		pathId: string;
		needleId?: string;
		needleScale?: number;
		class?: string;
	} = $props();
</script>

<!--
	Gentle, repeating vertical S-wave — the connective tissue of the thread
	between the illustrated "stations", so it never breaks off mid-page.
	Uses uniform ("slice") scaling rather than a stretched viewBox, so
	non-scaling-stroke geometry (path + needle) never distorts.
-->
<svg
	viewBox="0 0 40 400"
	preserveAspectRatio="xMidYMin slice"
	class={className}
	style="filter: drop-shadow(0 0 6px rgba(255, 255, 255, 0.9)) drop-shadow(0 2px 8px rgba(0, 0, 0, 0.35)) drop-shadow(0 0 12px rgba(200, 162, 122, 0.65));"
	aria-hidden="true"
	focusable="false"
>
	<path
		id={pathId}
		d="M20,0 C5,35 35,70 20,105 C5,140 35,175 20,210 C5,245 35,280 20,315 C10,340 30,365 20,400"
		fill="none"
		stroke="currentColor"
		stroke-width="4"
		vector-effect="non-scaling-stroke"
		stroke-linecap="round"
	/>
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

<script lang="ts">
	let { pathId, class: className = '' }: { pathId: string; class?: string } = $props();
</script>

<!--
	Gentle, repeating vertical S-wave — the connective tissue of the thread
	between the illustrated "stations", so it never breaks off mid-page.
-->
<svg
	viewBox="0 0 40 400"
	preserveAspectRatio="none"
	class={className}
	style="filter: drop-shadow(0 0 6px rgba(255, 255, 255, 0.9)) drop-shadow(0 2px 8px rgba(0, 0, 0, 0.35));"
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
</svg>

<script lang="ts">
	let { pathId, class: className = '' }: { pathId: string; class?: string } = $props();
</script>

<!--
	Single continuous-line silhouette of a gown on a hanger.
	Mirror-symmetric outline, one unbroken stroke starting/ending at the hook.
-->
<svg viewBox="0 0 200 320" class={className} aria-hidden="true" focusable="false">
	<path
		id={pathId}
		d="M 100.0,6.0 C 104.0,6.0 106.0,10.0 104.0,14.0 C 108.0,18.0 112.0,20.0 115.0,22.0 Q 128.0,26.0 135.0,34.0 C 140.0,42.0 145.0,50.0 142.0,60.0 C 138.0,80.0 128.0,105.0 125.0,130.0 C 130.0,150.0 140.0,170.0 145.0,190.0 C 155.0,230.0 168.0,265.0 175.0,300.0 Q 140.0,314.0 100.0,312.0 Q 60.0,314.0 25.0,300.0 C 32.0,265.0 45.0,230.0 55.0,190.0 C 60.0,170.0 70.0,150.0 75.0,130.0 C 72.0,105.0 62.0,80.0 58.0,60.0 C 55.0,50.0 60.0,42.0 65.0,34.0 Q 72.0,26.0 85.0,22.0 C 88.0,20.0 92.0,18.0 96.0,14.0 C 94.0,10.0 96.0,6.0 100.0,6.0"
		fill="none"
		stroke="currentColor"
		stroke-width="2.5"
		stroke-linecap="round"
		stroke-linejoin="round"
	/>
</svg>

<script lang="ts">
	import katex from 'katex';

	const { text } = $props();
	let outputDiv: HTMLDivElement;
	$effect(() => {
		katex.render(text, outputDiv, {
			throwOnError: false
		});
	});
</script>

<div bind:this={outputDiv}></div>

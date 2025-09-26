<script lang="ts">
	import Equation from './Equation.svelte';

	let textareaElement = $state<HTMLTextAreaElement>();

	let text = $state(
		'2 + 2 = 4\n4 - 1 = 3\n\\displaystyle x=\\frac{-b \\pm \\sqrt{b^2 - 4ac}}{2a}'
	);

	let disableMultiline = $state(false);
	let lines = $derived(
		disableMultiline ? [text] : text.split('\n').filter((line) => line.trim() !== '')
	);

	$effect(() => {
		if (textareaElement) {
			const hiddenDiv = document.getElementById('hidden-div');
			if (hiddenDiv) {
				hiddenDiv.style.width = `${textareaElement.clientWidth}px`;
				hiddenDiv.textContent = text + '\u200b'; // Add zero-width space to prevent jumpy behavior
				textareaElement.style.height = hiddenDiv.scrollHeight + 'px';
			}
		}
	});
</script>

<div class="container mx-auto min-h-screen p-4">
	<h1 class="mb-4 text-2xl font-bold">QuickMaths</h1>
	<p>
		This is a simple app that renders <a
			href="https://www.latex-project.org/about/"
			class="text-blue-500 underline visited:text-purple-500"
			target="_blank"
			rel="noopener noreferrer">LaTeX</a
		>
		style math expressions using
		<a
			href="https://katex.org/"
			class="text-blue-500 underline visited:text-purple-500"
			target="_blank"
			rel="noopener noreferrer">KaTeX</a
		>.
	</p>
	<div class="mb-4">
		<label for="math-input">Enter math expressions:</label>
		<textarea
			bind:this={textareaElement}
			class="min-h-24 w-full rounded border border-gray-300 p-2"
			bind:value={text}
			id="math-input"
		></textarea>
		<div id="hidden-div"></div>
		<div class="mt-2 flex items-center gap-2">
			<input
				type="checkbox"
				id="disable-multiline"
				bind:checked={disableMultiline}
				class="h-4 w-4"
			/>
			<label
				for="disable-multiline"
				title="When unchecked, each line is rendered in a separate context. Check this when you intend to use multiline tex commands like \begin"
				>Disable multiline <i>(?)</i></label
			>
		</div>
	</div>
	<div class="flex flex-col items-center justify-center gap-4">
		{#each lines as line}
			<Equation text={line} />
		{/each}
	</div>

	<div class="mt-8 text-center text-sm text-gray-500">
		<p>&copy; 2025 Lee Chengkai. All rights reserved.</p>
		<p>
			Browse the source on
			<a
				href="https://github.com/chunkily/quickmaths"
				class="text-blue-500 visited:text-purple-500"
				target="_blank"
				rel="noopener noreferrer"
			>
				<img src="/github-mark.svg" alt="GitHub Logo" class="mr-1 inline h-4 w-4" />
				GitHub
			</a>
		</p>
		<p>
			Built with
			<a href="https://svelte.dev/">SvelteKit.</a>
		</p>
	</div>
</div>

<style>
	#hidden-div {
		position: absolute;
		visibility: hidden;
		white-space: pre-wrap;
		word-wrap: break-word;
		width: 100%;
		padding: 10px;
		border: 1px solid #ccc;
		box-sizing: border-box;
	}
</style>

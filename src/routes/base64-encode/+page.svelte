<script lang="ts">
	import CopyIcon from '$lib/components/icons/copy.svelte';

	let inputText = $state(
		'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nunc lobortis erat vitae odio viverra, eget congue urna interdum. Nulla ac lorem et tellus scelerisque mollis. Pellentesque posuere augue vel metus sodales tempus.'
	);
	let encodedText = $state('');
	let errorMsg = $state('');

	$effect(() => {
		try {
			encodedText = btoa(inputText);
			errorMsg = '';
		} catch {
			encodedText = '';
			errorMsg = 'Encoding failed';
		}
	});

	const copyEncoded = () => {
		navigator.clipboard.writeText(encodedText);
	};
</script>

<svelte:head>
	<title>tools.karolyi.dev | base64 encoder</title>
	<meta name="description" content="A simple Base64 encoder" />
	<meta name="keywords" content="base64, encoder, base64 encoder" />
</svelte:head>

<div class="flex h-full w-full items-center justify-center">
	<main class="flex flex-col gap-16 text-sm md:text-base xl:flex-row">
		<div
			class="flex w-[calc(100vw-2rem)] flex-col justify-between border border-r-2 border-b-2 border-cyan-950 bg-zinc-50 font-mono md:w-xl"
		>
			<textarea
				class="w-[calc(100vw-2rem)] flex-1 resize-none p-4 focus-visible:outline-0 md:w-xl"
				bind:value={inputText}
			></textarea>
		</div>
		<div
			class="flex w-[calc(100vw-2rem)] flex-col justify-between border border-r-2 border-b-2 border-cyan-950 bg-zinc-50 md:min-h-64 md:w-xl"
		>
			<div class="relative flex-1">
				<pre
					aria-hidden="true"
					class="border-cyan-950 p-4 wrap-break-word break-keep whitespace-pre-wrap text-black">{encodedText}</pre>
				{#if !errorMsg}
					<button
						onclick={copyEncoded}
						class="absolute right-2 bottom-2 flex cursor-pointer items-center gap-1 border border-r-2 border-b-2 border-cyan-800 bg-zinc-50 p-1 text-cyan-800 active:border-t-zinc-50 active:border-l-zinc-50 active:bg-cyan-800 active:text-zinc-50"
					>
						<CopyIcon className="size-5" />
					</button>
				{/if}
			</div>
			{#if errorMsg}
				<div class="border-t border-cyan-950 bg-red-200 px-4 py-2">{errorMsg}</div>
			{:else if encodedText}
				<div class="border-t border-cyan-950 bg-green-200 px-4 py-2">Encoding successful</div>
			{/if}
		</div>
	</main>
</div>

<script lang="ts">
	import CopyIcon from '$lib/components/icons/copy.svelte';

	let inputText = $state(
		'TG9yZW0gaXBzdW0gZG9sb3Igc2l0IGFtZXQsIGNvbnNlY3RldHVyIGFkaXBpc2NpbmcgZWxpdC4gTnVuYyBsb2JvcnRpcyBlcmF0IHZpdGFlIG9kaW8gdml2ZXJyYSwgZWdldCBjb25ndWUgdXJuYSBpbnRlcmR1bS4gTnVsbGEgYWMgbG9yZW0gZXQgdGVsbHVzIHNjZWxlcmlzcXVlIG1vbGxpcy4gUGVsbGVudGVzcXVlIHBvc3VlcmUgYXVndWUgdmVsIG1ldHVzIHNvZGFsZXMgdGVtcHVzLg=='
	);
	let decodedText = $state('');
	let errorMsg = $state('');

	$effect(() => {
		try {
			decodedText = atob(inputText);
			errorMsg = '';
		} catch {
			decodedText = '';
			errorMsg = 'Decoding failed';
		}
	});

	const copyDecoded = () => {
		navigator.clipboard.writeText(decodedText);
	};
</script>

<svelte:head>
	<title>tools.karolyi.dev | base64 decoder</title>
	<meta name="description" content="A simple Base64 decoder" />
	<meta name="keywords" content="base64, decoder, base64 decoder" />
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
					class="border-cyan-950 p-4 wrap-break-word break-keep whitespace-pre-wrap text-black">{decodedText}</pre>
				{#if !errorMsg}
					<button
						onclick={copyDecoded}
						class="absolute right-2 bottom-2 flex cursor-pointer items-center gap-1 border border-r-2 border-b-2 border-cyan-800 bg-zinc-50 p-1 text-cyan-800 active:border-t-zinc-50 active:border-l-zinc-50 active:bg-cyan-800 active:text-zinc-50"
					>
						<CopyIcon className="size-5" />
					</button>
				{/if}
			</div>
			{#if errorMsg}
				<div class="border-t border-cyan-950 bg-red-200 px-4 py-2">{errorMsg}</div>
			{:else if decodedText}
				<div class="border-t border-cyan-950 bg-green-200 px-4 py-2">Decoding successful</div>
			{/if}
		</div>
	</main>
</div>

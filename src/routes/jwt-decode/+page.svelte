<script lang="ts">
	import * as jose from 'jose';
	import hljs from 'highlight.js/lib/core';
	import json from 'highlight.js/lib/languages/json';
	import 'highlight.js/styles/github.css';

	hljs.registerLanguage('json', json);

	let inputJwt = $state(
		'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI3MDQ1Njg0ZC1lOGJiLTQ2Y2UtYjdlYS1lYmY1ZWJkODQyNzciLCJuYW1lIjoiUGVsZGEgR2V6YSIsImlzQWRtaW4iOnRydWUsImlhdCI6MTMzNzk4NzAwMH0.ZOY3HVPVMLz4xg-6ur6eAyWFdavI_2KKEvAFr32yL2k'
	);
	let inputSecret = $state('super-non-secret-example-secret-1');
	let decodedJwt = $state<jose.JWTPayload | null>({});
	let isSignatureValid = $state(true);

	let splitJwt = $derived.by(() => {
		const match = inputJwt.match(/^([^.]+)(\.)([^.]*)(\.)([^.]*)(.*)$/);
		return match ?? [inputJwt, inputJwt];
	});

	$effect(() => {
		const decode = async (jwt: string, secret: string) => {
			try {
				const encSecret = new TextEncoder().encode(secret);
				const verifiedJwt = await jose.jwtVerify(jwt, encSecret);
				decodedJwt = verifiedJwt.payload;
				isSignatureValid = true;
			} catch {
				isSignatureValid = false;
				try {
					decodedJwt = jose.decodeJwt(jwt);
				} catch {
					decodedJwt = null;
				}
			}
		};

		decode(inputJwt, inputSecret);
	});

	const highlightedJwt = $derived(
		hljs.highlight(JSON.stringify(decodedJwt, null, 2), { language: 'json' }).value
	);
</script>

<svelte:head>
	<title>tools.karolyi.dev | jwt decoder</title>
	<meta name="description" content="A simple JWT decoder" />
	<meta name="keywords" content="jwt, jws, decoder, jwt decoder" />
</svelte:head>

<div class="flex h-full w-full items-center justify-center">
	<main class="flex flex-col gap-16 text-sm md:text-base xl:flex-row">
		<div
			class="flex w-[calc(100vw-2rem)] flex-col justify-between border border-cyan-950 bg-zinc-50 font-mono md:w-xl"
		>
			<div class="relative flex-1">
				<textarea
					class="absolute top-[-1px] left-[-1px] h-full w-[calc(100vw-2rem)] resize-none p-4 focus-visible:outline-0 md:w-xl"
					style="-webkit-text-fill-color: transparent"
					bind:value={inputJwt}
				></textarea>
				<pre
					aria-hidden="true"
					class="h-full border-r border-cyan-950 p-4 wrap-break-word break-keep whitespace-pre-wrap text-black"><span
						class="text-[#005cc5]">{splitJwt[1]}</span
					><span class="text-black">{splitJwt[2]}</span><span class="text-cyan-950"
						>{splitJwt[3]}</span
					><span class="text-black">{splitJwt[4]}</span><span class="text-[#d73a49]"
						>{splitJwt[5]}</span
					><span class="text-[#6f42c1]">{splitJwt[6]}</span></pre>
			</div>
			<input
				class="border-t border-r border-b border-cyan-950 px-4 py-2 focus-visible:outline-0"
				bind:value={inputSecret}
			/>
		</div>
		<div
			class="flex w-[calc(100vw-2rem)] flex-col justify-between border border-r-2 border-b-2 border-cyan-950 bg-zinc-50 md:min-h-64 md:w-xl"
		>
			<pre class="flex-1 p-4">{@html highlightedJwt}</pre>
			{#if decodedJwt === null}
				<div class="border-t border-cyan-950 bg-red-200 px-4 py-2">Invalid JWT</div>
			{:else if !isSignatureValid}
				<div class="border-t border-cyan-950 bg-red-200 px-4 py-2">Signature invalid</div>
			{:else}
				<div class="border-t border-cyan-950 bg-green-200 px-4 py-2">Signature valid</div>
			{/if}
		</div>
	</main>
</div>

<script lang="ts">
	import * as jose from 'jose';
	import hljs from 'highlight.js/lib/core';
	import json from 'highlight.js/lib/languages/json';
	import 'highlight.js/styles/github.css';

	hljs.registerLanguage('json', json);

	let inputJwt = $state(
		'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJleGFtcGxlLXN1YiIsIm5hbWUiOiJFeGFtcGxlIE5hbWUiLCJpc0FkbWluIjp0cnVlLCJpYXQiOjEzNTU5NTgwMDB9.q2Nn2h3zgcFbyn4_ejfH6BSjyKuMeNeqNZ7CpcGfm28'
	);
	let inputSecret = $state('test-secret-that-is-very-long-yeah');
	let decodedJwt = $state<jose.JWTPayload | null>({});
	let isSignatureValid = $state(true);

	let choppedJwt = $derived.by(() => {
		const [header, payload, signature, ...rest] = inputJwt.split('.');
		return [header, payload, signature, rest.join('.')];
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
						class="text-[#005cc5]">{choppedJwt[0]}</span
					>.<span class="text-cyan-950">{choppedJwt[1]}</span>.<span class="text-[#d73a49]"
						>{choppedJwt[2]}</span
					>{#if choppedJwt[3]}.<span class="text-cyan-950">{choppedJwt[3]}</span>{/if}</pre>
			</div>
			<input
				class="border-t border-r border-b border-cyan-950 px-4 py-2 focus-visible:outline-0"
				bind:value={inputSecret}
			/>
		</div>
		<div
			class="flex w-[calc(100vw-2rem)] flex-col justify-between border border-r-2 border-b-2 border-cyan-950 bg-zinc-50 md:min-h-64 md:w-xl"
		>
			<pre class="p-4">{@html highlightedJwt}</pre>
			{#if isSignatureValid}
				<div class="border-t border-cyan-950 bg-green-200 px-4 py-2">Signature valid</div>
			{:else}
				<div class="border-t border-cyan-950 bg-red-200 px-4 py-2">Signature invalid</div>
			{/if}
		</div>
	</main>
</div>

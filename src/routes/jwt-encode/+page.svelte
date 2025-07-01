<script lang="ts">
	import * as jose from 'jose';
	import hljs from 'highlight.js/lib/core';
	import json from 'highlight.js/lib/languages/json';
	import 'highlight.js/styles/github.css';

	hljs.registerLanguage('json', json);

	let inputHeader = $state(JSON.stringify({ alg: 'HS256', typ: 'JWT' }, null, 2));
	let inputPayload = $state(
		JSON.stringify(
			{
				sub: '7045684d-e8bb-46ce-b7ea-ebf5ebd84277',
				name: 'Pelda Geza',
				isAdmin: true,
				iat: 1337987000
			},
			null,
			2
		)
	);
	let inputSecret = $state('super-non-secret-example-secret-1');
	let encodedJwt = $state('');
	let errorMsg = $state('');

	$effect(() => {
		const encode = async (header: string, payload: string, secret: string) => {
			try {
				const encSecret = new TextEncoder().encode(secret);
				const parsedHeader = JSON.parse(header);
				const parsedPayload = payload.length > 0 ? JSON.parse(payload) : undefined;
				const jwt = await new jose.SignJWT(parsedPayload)
					.setProtectedHeader(parsedHeader)
					.sign(encSecret);
				encodedJwt = jwt;
				errorMsg = '';
			} catch (e) {
				encodedJwt = '';
				errorMsg = e instanceof Error ? e.message : String(e);
			}
		};

		if (inputSecret.length === 0) {
			errorMsg = 'Secret cannot be empty';
			return;
		}

		encode(inputHeader, inputPayload, inputSecret);
	});

	const highlightedPayload = $derived(hljs.highlight(inputPayload, { language: 'json' }).value);

	let choppedJwt = $derived.by(() => {
		const [header, payload, signature, ...rest] = encodedJwt.split('.');
		return [header, payload, signature, rest.join('.')];
	});
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
					bind:value={inputPayload}
				></textarea>
				<pre
					aria-hidden="true"
					class="h-full border-r border-cyan-950 p-4 text-black">{@html highlightedPayload}</pre>
			</div>
			<input
				class="border-t border-r border-b border-cyan-950 px-4 py-2 focus-visible:outline-0"
				bind:value={inputSecret}
			/>
		</div>
		<div
			class="flex w-[calc(100vw-2rem)] flex-col justify-between border border-r-2 border-b-2 border-cyan-950 bg-zinc-50 md:min-h-64 md:w-xl"
		>
			{#if errorMsg}
				<pre class="h-full"></pre>
				<div class="border-t border-cyan-950 bg-red-200 px-4 py-2">{errorMsg}</div>
			{:else}
				<pre
					aria-hidden="true"
					class="h-full p-4 wrap-break-word break-keep whitespace-pre-wrap text-black"><span
						class="text-[#005cc5]">{choppedJwt[0]}</span
					>.<span class="text-cyan-950">{choppedJwt[1]}</span>.<span class="text-[#d73a49]"
						>{choppedJwt[2]}</span
					>{#if choppedJwt[3]}.<span class="text-cyan-950">{choppedJwt[3]}</span>{/if}</pre>
				<div class="border-t border-cyan-950 bg-green-200 px-4 py-2">Encoding successful</div>
			{/if}
		</div>
	</main>
</div>

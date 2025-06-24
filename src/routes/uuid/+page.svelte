<script lang="ts">
	import ClipboardDocumentIcon from '$lib/components/icons/clipboard-document.svelte';
	import ClipboardDocumentCheckIcon from '$lib/components/icons/clipboard-document-check.svelte';
	import ArrowPath from '$lib/components/icons/arrow-path.svelte';
	import clsx from 'clsx';

	let uuid = $state(crypto.randomUUID());

	let copied = $state(false);
	let copiedTimeout: number | null = $state(null);
	let rotating = $state(false);
	let rotatingTimeout: number | null = $state(null);

	const generateUUID = () => {
		uuid = crypto.randomUUID();

		rotating = true;
		if (rotatingTimeout) clearTimeout(rotatingTimeout);
		rotatingTimeout = setTimeout(() => {
			rotating = false;
			rotatingTimeout = null;
		}, 500);
	};

	const copyUUID = () => {
		navigator.clipboard.writeText(uuid);

		copied = true;
		if (copiedTimeout) clearTimeout(copiedTimeout);
		copiedTimeout = setTimeout(() => {
			copied = false;
			copiedTimeout = null;
		}, 500);
	};
</script>

<div class="flex h-full w-full items-center justify-center">
	<main class="border border-cyan-950 p-4">
		<div class="flex flex-col items-center justify-between gap-4 md:flex-row md:gap-16">
			<code class="text-sm whitespace-pre md:text-xl">{uuid}</code>
			<div
				class="flex w-full flex-row-reverse justify-start gap-8 md:flex-row md:justify-around md:gap-2"
			>
				<button
					onclick={copyUUID}
					class="flex cursor-pointer items-center gap-1 border border-cyan-800 bg-zinc-50 p-1 text-cyan-800 transition-all duration-200 hover:bg-sky-800 hover:text-zinc-50"
				>
					{#if copied}
						<ClipboardDocumentCheckIcon className="size-5" />
					{:else}
						<ClipboardDocumentIcon className="size-5" />
					{/if}
				</button>
				<button
					class="flex cursor-pointer items-center gap-1 border border-cyan-800 bg-zinc-50 p-1 text-cyan-800 transition-all duration-200 hover:bg-sky-800 hover:text-zinc-50"
					onclick={generateUUID}
				>
					<ArrowPath
						className={clsx(
							'size-5',
							rotating && 'animate-[rotate_0.5s_cubic-bezier(0.4,0,0.2,1)]'
						)}
					/>
				</button>
			</div>
		</div>
	</main>
</div>

<style>
	@keyframes -global-rotate {
		0% {
			transform: rotate(0deg);
		}
		95% {
			transform: rotate(180deg);
		}
		100% {
			transform: rotate(180deg);
		}
	}
</style>

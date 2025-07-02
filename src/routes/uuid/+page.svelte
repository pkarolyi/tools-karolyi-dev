<script lang="ts">
	import CopyIcon from '$lib/components/icons/copy.svelte';
	import ReturnIcon from '$lib/components/icons/return.svelte';

	let uuid = $state(crypto.randomUUID());

	const generateUUID = () => {
		uuid = crypto.randomUUID();
	};

	const copyUUID = () => {
		navigator.clipboard.writeText(uuid);
	};
</script>

<svelte:head>
	<title>tools.karolyi.dev | uuid 4 generator</title>
	<meta name="description" content="A simple UUID v4 generator" />
	<meta name="keywords" content="uuid, uuidv4, generator, uuid generator" />
</svelte:head>

<div class="flex h-full w-full items-center justify-center">
	<main class="border border-r-2 border-b-2 border-cyan-950 bg-zinc-50 p-4">
		<div class="flex flex-col items-center justify-between gap-4 sm:flex-row sm:gap-16">
			<code
				class="border-t-2 border-transparent text-xs leading-none whitespace-pre sm:text-lg md:text-xl"
				>{uuid}</code
			>
			<div
				class="flex w-full flex-row-reverse justify-start gap-8 sm:flex-row sm:justify-around sm:gap-4 md:gap-2"
			>
				<button
					onclick={copyUUID}
					class="flex cursor-pointer items-center gap-1 border border-r-2 border-b-2 border-cyan-800 bg-zinc-50 p-1 text-cyan-800 active:border-t-zinc-50 active:border-l-zinc-50 active:bg-cyan-800 active:text-zinc-50"
				>
					<CopyIcon className="size-5" />
				</button>
				<button
					class="flex cursor-pointer items-center gap-1 border border-r-2 border-b-2 border-cyan-800 bg-zinc-50 p-1 text-cyan-800 active:border-t-zinc-50 active:border-l-zinc-50 active:bg-cyan-800 active:text-zinc-50"
					onclick={generateUUID}
				>
					<ReturnIcon className="size-5" />
				</button>
			</div>
		</div>
	</main>
</div>

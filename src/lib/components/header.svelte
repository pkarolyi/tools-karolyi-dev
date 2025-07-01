<script lang="ts">
	import { page } from '$app/state';
	import clsx from 'clsx';
	import GithubIcon from './icons/github.svelte';

	const path = $derived(page.url.pathname);
</script>

<header
	class="flex w-full flex-col items-center gap-2 border-cyan-950 bg-cyan-800 px-4 text-zinc-100 md:flex-row md:gap-10"
>
	<h1 class="py-2">tools.karolyi.dev</h1>
	<nav class="flex flex-1 gap-6">
		<a href="/uuid" class={clsx('py-2', path === '/uuid' && 'underline')}>uuid-4-generator</a>
		<a href="/jwt-decode" class={clsx('py-2', path === '/jwt-decode' && 'underline')}>
			jwt-decoder
		</a>
		<a href="/jwt-encode" class={clsx('py-2', path === '/jwt-encode' && 'underline')}>
			jwt-encoder
		</a>
	</nav>
	<a href="https://github.com/pkarolyi/tools-karolyi-dev"><GithubIcon className="size-5" /></a>
</header>

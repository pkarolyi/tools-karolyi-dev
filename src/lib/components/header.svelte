<script lang="ts">
	import { page } from '$app/state';
	import clsx from 'clsx';

	const path = $derived(page.url.pathname);
</script>

<header
	class="flex w-full flex-col gap-2 border-cyan-950 bg-cyan-800 px-4 text-zinc-100 md:flex-row md:gap-10"
>
	<h1 class="py-2">tools.karolyi.dev</h1>
	<nav class="flex gap-6">
		<a href="/uuid" class={clsx('py-2', path === '/uuid' && 'underline')}>uuid-4-generator</a>
		<a href="/jwt-decode" class={clsx('py-2', path === '/jwt-decode' && 'underline')}>
			jwt-decoder
		</a>
		<a href="/jwt-encode" class={clsx('py-2', path === '/jwt-encode' && 'underline')}>
			jwt-encoder
		</a>
	</nav>
</header>

<script lang="ts">
	import { page } from '$app/state';
	import clsx from 'clsx';

	const path = $derived(page.url.pathname);
</script>

<nav class="flex w-full gap-8 px-4 py-2">
	<h1>tools.karolyi.dev</h1>
	<ul class="flex gap-4">
		<li>
			<a href="/uuid" class="group text-cyan-800">
				uuid-4-generator
				<span
					class={clsx(
						'block h-[1px] bg-cyan-800 opacity-0 transition-all duration-200 group-hover:opacity-100',
						path === '/uuid' && 'opacity-100!'
					)}
				></span>
			</a>
		</li>
		<li>
			<a href="/jwt" class="group text-cyan-800">
				jwt-encode-decode
				<span
					class={clsx(
						'block h-[1px] bg-cyan-800 opacity-0 transition-all duration-200 group-hover:opacity-100',
						path === '/jwt' && 'opacity-100!'
					)}
				></span>
			</a>
		</li>
	</ul>
</nav>

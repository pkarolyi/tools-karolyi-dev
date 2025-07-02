<script lang="ts">
	import { injectAnalytics } from '@vercel/analytics/sveltekit';
	import Footer from '$lib/components/footer.svelte';
	import Header from '$lib/components/header.svelte';
	import '../app.css';

	injectAnalytics();

	let { children } = $props();
</script>

<div
	class="flex h-screen w-screen flex-col bg-zinc-200"
	style="-webkit-font-smoothing: antialiased;-moz-osx-font-smoothing: grayscale"
>
	<Header />
	{@render children()}
	<Footer />
</div>

<script lang="ts">
	import Footer from '$lib/components/footer.svelte';
	import Header from '$lib/components/header.svelte';
	import '../app.css';

	let { children } = $props();
</script>

<div class="flex h-screen w-screen flex-col bg-zinc-200">
	<Header />
	{@render children()}
	<Footer />
</div>

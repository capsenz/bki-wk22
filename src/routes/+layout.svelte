<script lang="ts">
	import '../app.css';
	import Header from '$lib/components/layout/Header.svelte';
	// import Header from './Header.svelte';
	// import Footer from './Footer.svelte';

	let { children } = $props();
</script>

<div class="bg-background-color flex min-h-screen flex-col">
	<Header />
	{@render children()}
</div>

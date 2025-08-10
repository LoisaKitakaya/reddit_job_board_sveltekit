<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';
	import Navbar from '$lib/components/Navbar.svelte';
	import Footer from '$lib/components/Footer.svelte';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<title>Reddit Job Board</title>
</svelte:head>

<div>
	<div class="mx-auto w-full lg:w-3/4">
		<Navbar />

		<div class="min-h-screen bg-gray-800 px-4 py-4">
			{@render children?.()}
		</div>

		<Footer />
	</div>
</div>

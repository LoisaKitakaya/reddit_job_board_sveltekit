<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';
	import Navbar from '$lib/components/Navbar.svelte';
	import Footer from '$lib/components/Footer.svelte';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<title>Reddit Freelancer</title>
</svelte:head>

<div class="bg-amber-50">
	<div class="mx-auto w-3/4">
		<Navbar />

		<div class="py-4 min-h-screen px-4 bg-gray-800">
			{@render children?.()}
		</div>

		<Footer />
	</div>
</div>

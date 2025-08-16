<script lang="ts">
	import { page } from '$app/state';
</script>

<nav class="sticky top-0 z-10 border-b border-gray-700 bg-gray-800 text-white shadow-md">
	<ul class="mx-auto flex max-w-7xl flex-row items-center gap-4 px-4 py-4 sm:px-6 lg:px-8">
		<li>
			<a
				href="/"
				class={`text-lg font-semibold transition-colors duration-200 ${page.url.pathname === '/' ? 'text-red-400' : 'text-white'} hover:text-red-200`}
				>Home</a
			>
		</li>
		<li>
			<a
				href="/about"
				class={`text-lg font-semibold transition-colors duration-200 ${page.url.pathname === '/about' ? 'text-red-400' : 'text-white'} hover:text-red-200`}
				>About</a
			>
		</li>
	</ul>
</nav>

<script lang="ts">
	export let postOwner: string = '';
	export let postTrigger: string = '';
	export let subreddit: string = '';

	$: filterMessage = (() => {
		const filters = [];

		if (postOwner) filters.push(`username "${postOwner}"`);
		if (postTrigger) filters.push(`post type "${postTrigger}"`);
		if (subreddit) filters.push(`subreddit "${subreddit}"`);

		return filters.length > 0 ? ` for ${filters.join(', ')}` : '';
	})();
</script>

<div class="error-message w-full border-y py-72 text-center">
	<div
		class="relative flex flex-col items-center justify-center rounded border border-red-400 bg-red-100 px-4 py-3 text-red-700"
		role="alert"
	>
		<div class="mb-8 text-2xl font-bold">
			<p>No Results Found!</p>
		</div>

		<div class="text-lg">
			<p class="block sm:inline">
				Sorry, we couldn't find any posts {filterMessage}.

				<br />
				<br />

				Try adjusting your filters or clearing them to see more results.
			</p>
		</div>
	</div>
</div>

<style lang="css">
	.error-message {
		min-height: 200px;
	}
</style>

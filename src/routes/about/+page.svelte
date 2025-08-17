<script lang="ts">
	export let data = {
		title: "About Our Job Platform",
		description: "Learn about Reddit Job Aggregator, a platform dedicated to connecting freelancers with opportunities by aggregating and categorizing jobs from Reddit."
	};
</script>

<svelte:head>
	<title>{page.data.title} | Reddit Job Board</title>

	<meta name="description" content="{page.data.description}">
</svelte:head>

<h1>About <span class="font-semibold text-red-400">Reddit Job Board</span></h1>

<div class="py-4">
	<p class="mb-4">
		This is a tool designed to empower freelancers by simplifying the search for opportunities on
		Reddit. By aggregating posts from subreddits like r/slavelabour, r/PythonJobs, and others, it
		help you discover tasks and offers that match your skills, whether you're a developer, designer,
		or writer.
	</p>

	<p class="mb-4">
		<span class="font-semibold text-red-400">Reddit Job Board</span> was created by
		<a
			href="https://loisakitakaya.info"
			class="font-medium text-blue-400 transition-colors duration-200 hover:text-blue-300"
		>
			Loisa Kitakaya
		</a>, a passionate developer dedicated to building tools that make freelancing easier.
	</p>
</div>

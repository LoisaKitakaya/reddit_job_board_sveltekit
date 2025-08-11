<script lang="ts">
	import moment from 'moment';
	import { onMount } from 'svelte';
	import { env } from '$env/dynamic/public';
	import { ArrowRight, ArrowLeft } from '@lucide/svelte';
	import ErrorMessage from '$lib/components/ErrorMessage.svelte';

	interface Post {
		id: string;
		post_owner_reddit_username: string;
		reddit_post_id: string;
		post_url: string;
		post_title: string;
		post_category: string | null;
		post_trigger: string | null;
		subreddit: string;
		post_time: string | null;
	}

	let posts: Post[] = [];
	let totalCount: number = 0;
	let limit: number = 20;
	let offset: number = 0;
	let postOwner: string = '';
	let postTrigger: string = '';
	let subreddit: string = '';
	let isLoading: boolean = false;

	const triggerOptions = [
		{ value: '', label: 'All Types' },
		{ value: 'task', label: 'Task' },
		{ value: 'offer', label: 'Offer' }
	];

	const subredditOptions = [
		{ value: '', label: 'All Subreddits' },
		{ value: 'slavelabour', label: 'r/slavelabour' },
		{ value: 'WebDeveloperJobs', label: 'r/WebDeveloperJobs' },
		{ value: 'WebDevJobs', label: 'r/WebDevJobs' },
		{ value: 'DeveloperJobs', label: 'r/DeveloperJobs' },
		{ value: 'forhire', label: 'r/forhire' },
		{ value: 'freelance_forhire', label: 'r/freelance_forhire' },
		{ value: 'programmers_forhire', label: 'r/programmers_forhire' },
		{ value: 'DoneDirtCheap', label: 'r/DoneDirtCheap' },
		{ value: 'VirtualAssistant', label: 'r/VirtualAssistant' },
		{ value: 'B2BForHire', label: 'r/B2BForHire' },
		{ value: 'jobbit', label: 'r/jobbit' },
		{ value: 'remotedaily', label: 'r/remotedaily' },
		{ value: 'PythonJobs', label: 'r/PythonJobs' },
		{ value: 'remotejs', label: 'r/remotejs' },
		{ value: 'CodingJobs', label: 'r/CodingJobs' },
		{ value: 'remotepython', label: 'r/remotepython' }
	];

	$: currentPage = Math.floor(offset / limit) + 1;
	$: totalPages = Math.ceil(totalCount / limit);

	async function fetchPosts() {
		isLoading = true;

		const params = new URLSearchParams();

		if (postOwner) params.append('post_owner', postOwner);
		if (postTrigger) params.append('post_trigger', postTrigger);
		if (subreddit) params.append('subreddit', subreddit);

		params.append('limit', limit.toString());
		params.append('offset', offset.toString());

		const url = `${env.PUBLIC_BACKEND_URL}/api/reddit_posts?${params.toString()}`;

		try {
			const response = await fetch(url);

			const data = await response.json();

			posts = data.posts;

			totalCount = data.total_count;
		} catch (error) {
			console.error('Fetch error:', error);
		} finally {
			isLoading = false;
		}
	}

	function handleFilterChange() {
		offset = 0; // Reset offset when filters change
		fetchPosts();
	}

	function prevPage() {
		if (offset >= limit) {
			offset -= limit;
			fetchPosts();
		}
	}

	function nextPage() {
		if (offset + limit < totalCount) {
			offset += limit;
			fetchPosts();
		}
	}

	onMount(fetchPosts);
</script>

<h1>Welcome to <span class="font-semibold text-red-400">Reddit Job Board</span></h1>

<div class="filters pt-4 pb-6">
	<div
		class="flex w-full flex-col items-start justify-start lg:flex-row lg:items-center lg:justify-between"
	>
		<div>
			<label class="mb-1 label-text" for="postOwner">Post Owner</label>
			<input
				id="postOwner"
				type="text"
				bind:value={postOwner}
				on:input={handleFilterChange}
				placeholder="Enter Username"
				class="input mb-4 w-64 border lg:mb-0 lg:w-full"
			/>
		</div>
		<div>
			<label class="label w-full pb-1">
				<span class="label-text">Post Type</span>
				<select
					bind:value={postTrigger}
					on:change={handleFilterChange}
					class="select mb-4 w-64 border lg:mb-0 lg:w-full"
				>
					{#each triggerOptions as option}
						<option value={option.value}>{option.label}</option>
					{/each}
				</select>
			</label>
		</div>
		<div>
			<label class="label w-full pb-1">
				<span class="label-text">Subreddit</span>
				<select
					bind:value={subreddit}
					on:change={handleFilterChange}
					class="select w-64 border lg:w-full"
				>
					{#each subredditOptions as option}
						<option value={option.value}>{option.label}</option>
					{/each}
				</select>
			</label>
		</div>
	</div>
</div>

{#if isLoading === true}
	<div class="flex items-center justify-center border-y border-gray-400 py-72">
		<span class="loader"></span>
	</div>
{:else if isLoading === false && posts.length > 0}
	<div class="table-wrap border-y border-gray-400">
		<table class="table caption-bottom">
			<thead>
				<tr class="border-b border-gray-700">
					<th class="font-semibold">#</th>
					<!-- <th>Post ID</th> -->
					<th>Post Title</th>
					<!-- <th>Post Category</th> -->
					<th>Post Type</th>
					<th>Post Link</th>
					<th>Reddit Username</th>
					<th>Subreddit</th>
					<th>Time Posted</th>
				</tr>
			</thead>
			<tbody class="[&>tr]:hover:preset-tonal-primary">
				{#each posts as post, index}
					<tr>
						<th class="font-semibold">{index + 1}</th>
						<!-- <td>{post.reddit_post_id}</td> -->
						<td>{post.post_title}</td>
						<!-- <td>{post.post_category}</td> -->
						<td>
							{#if post.post_trigger === 'task'}
								Task
							{:else if post.post_trigger === 'offer'}
								Offer
							{/if}
						</td>
						<td
							><a class="link text-red-400 hover:underline" target="_blank" href={post.post_url}
								>View on Reddit</a
							></td
						>
						<td
							><a
								class="link text-red-400 hover:underline"
								target="_blank"
								href={`https://reddit.com/user/${post.post_owner_reddit_username}`}
								>u/{post.post_owner_reddit_username}</a
							></td
						>
						<td
							><a
								class="link text-red-400 hover:underline"
								target="_blank"
								href={`https://reddit.com/r/${post.subreddit}`}>r/{post.subreddit}</a
							></td
						>
						<td>{moment(post.post_time).fromNow()}</td>
					</tr>
				{/each}
			</tbody>
		</table>
	</div>
{:else if isLoading === false && posts.length === 0}
	<ErrorMessage {postOwner} {postTrigger} {subreddit} />
{/if}

<div class="pagination flex items-center justify-center gap-4 pt-8 pb-4">
	<button
		on:click={prevPage}
		disabled={offset === 0}
		type="button"
		class="btn preset-filled btn-sm"
	>
		<ArrowLeft />
	</button>
	<span>Page {currentPage} of {totalPages} | Total Posts: {totalCount}</span>
	<button
		on:click={nextPage}
		disabled={offset + limit >= totalCount}
		type="button"
		class="btn preset-filled btn-sm"
	>
		<ArrowRight />
	</button>
</div>

<style lang="css">
	.loader {
		position: relative;
		width: 100px;
		height: 100px;
	}

	.loader:before,
	.loader:after {
		content: '';
		border-radius: 50%;
		position: absolute;
		inset: 0;
		box-shadow: 0 0 10px 2px rgba(0, 0, 0, 0.3) inset;
	}
	.loader:after {
		box-shadow: 0 2px 0 rgb(199, 44, 44) inset;
		animation: rotate 2s linear infinite;
	}

	@keyframes rotate {
		0% {
			transform: rotate(0);
		}
		100% {
			transform: rotate(360deg);
		}
	}
</style>

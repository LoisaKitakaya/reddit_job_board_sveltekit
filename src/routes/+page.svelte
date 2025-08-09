<script lang="ts">
	import { onMount } from 'svelte';

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

	let isLoading: boolean = false;

	async function fetchPosts(url: string = 'http://localhost:8000/api/reddit_posts') {
		isLoading = true;

		try {
			const response = await fetch(url);

			const data = await response.json();

			posts = data;
		} catch (error) {
			console.error('Fetch error:', error);
		} finally {
			isLoading = false;
		}
	}

	onMount(fetchPosts);
</script>

<h1>Welcome to the <span class="font-semibold text-red-400">Reddit Freelancer</span></h1>

{#if isLoading === true}
	<div class="flex items-center justify-center pt-56">
		<span class="loader"></span>
	</div>
{:else if isLoading === false && posts.length > 0}
	<div class="py-4">
		{JSON.stringify(posts)}
	</div>
{/if}

<style lang="css">
	.loader{
    position: relative;
    font-size: 36px;
    letter-spacing: 2px;
  }
  .loader:before {
    content: "Loading";
    color: #fff;
  }
  .loader:after {
    content: "";
    width: 20px;
    height: 20px;
    background-color: red;
    border-radius: 50%;
    position: absolute;
    inset: 0;
    margin: auto;
    top: -70px;
    animation: motion 3s ease-in-out infinite;
  }

  @keyframes motion {
    0%, 50%, 100% {
      transform: translateX(0) scale(1);
    }
    25% {
      transform: translateX(-100px) scale(0.3);
    }
    75% {
      transform: translateX(100px) scale(0.3);
    }
  }
</style>

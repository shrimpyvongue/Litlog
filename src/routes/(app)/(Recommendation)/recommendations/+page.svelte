<script>
	/** @type {import('./$types').PageData} */
	import { enhance } from '$app/forms';
	import { page } from '$app/stores';
	export let data;
	let { recommendations } = data;
</script>

<div class="container">
	<div class="head">
		<h1>Recommendations</h1>
		<a href="/myrecommendation">Submit a recommendation?</a>
	</div>

	<div class="content">
		{#each recommendations as recommendations}
			<div class="card">
				<form method="post">
					<input type="hidden" name="id" bind:value={recommendations.id} />
					<div class="votes">
						{#if recommendations.Like[0]?.User?.some((user) => user?.id === $page.data?.user?.id)}
							<button formaction="?/like" style="color:#ff4500">
								<span class="material-icons"> arrow_upward </span>
							</button>
						{:else}
							<button formaction="?/like">
								<span class="material-icons"> arrow_upward </span>
							</button>
						{/if}
						{#if recommendations.Like.length && recommendations.disLike.length != 0}
							{#if recommendations.Like[0]?.User?.length - recommendations.disLike[0]?.User?.length != 0}
								<div class="voteCount">
									{recommendations.Like[0]?.User?.length -
										recommendations.disLike[0]?.User?.length >
									0
										? recommendations.Like[0]?.User?.length -
										  recommendations.disLike[0]?.User?.length
										: '-' +
										  recommendations.Like[0]?.User?.length -
										  recommendations.disLike[0]?.User?.length}
								</div>
							{/if}
						{:else if recommendations.Like.length == 0 && recommendations.disLike[0]?.User?.length > 0}
							-{recommendations.disLike[0]?.User?.length}
						{:else if recommendations.disLike.length == 0 && recommendations.Like[0]?.User?.length > 0}
							{recommendations.Like[0]?.User?.length}
						{/if}
						{#if recommendations.disLike[0]?.User?.some((user) => user?.id === $page.data?.user?.id)}
							<button formaction="?/dislike" style="color:#7193ff">
								<span class="material-icons"> arrow_downward </span>
							</button>
						{:else}
							<button formaction="?/dislike">
								<span class="material-icons"> arrow_downward </span>
							</button>
						{/if}
					</div>
				</form>
				<div class="titleCover">
					<!-- Book cover source -->
					<div class="imageContainer">
						<img
							src={'https://covers.openlibrary.org/b/olid/' +
								recommendations.cover1 +
								'-M.jpg?default=false'}
							onerror="this.onerror=null;this.src='http://covers.openlibrary.org/b/id/' +
						{recommendations.cover1} +
						'-M.jpg?default=false';"
							alt={recommendations.title1}
						/>
					</div>
					<a href="/books/{recommendations.book1Id}">
						<div class="title">{recommendations.title1}</div>
					</a>
				</div>
				<div class="titleCover">
					<!-- Book cover source -->
					<div class="imageContainer">
						<img
							src={'https://covers.openlibrary.org/b/olid/' +
								recommendations.cover2 +
								'-M.jpg?default=false'}
							onerror="this.onerror=null;this.src='http://covers.openlibrary.org/b/id/' +
					{recommendations.cover2} +
					'-M.jpg?default=false';"
							alt={recommendations.title2}
						/>
					</div>
					<a href="/books/{recommendations.book2Id}">
						<div class="title">{recommendations.title2}</div>
					</a>
				</div>
			</div>
		{/each}
	</div>
</div>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Caveat&family=Istok+Web&family=Poppins:wght@300&display=swap%27');
	* {
		font-family: 'Overpass', sans-serif;
		text-decoration: none;
		outline: none;
	}

	.material-icons {
		font-family: 'Material Icons';
	}

	:root {
		background-color: #edf1f5;
		color: #5c728a;
		overflow-y: scroll; /* Always show the vertical scroll bar */
	}

	/* Hide the vertical scroll bar */
	::-webkit-scrollbar {
		width: 0.5em;
		background-color: #edf1f5;
	}

	::-webkit-scrollbar-thumb {
		background-color: #5c728a;
	}

	::-webkit-scrollbar-thumb:hover {
		background-color: #1faafa;
	}

	.container {
		margin: 3% 15%;
	}

	.content {
		padding: 1rem 0rem;
		color: #647380;
		display: flex;
		flex-wrap: wrap; /* Allow flex items to wrap to a new line */
		justify-content: start; /* Distribute items evenly between rows */
	}

	.votes {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.votes span {
		font-weight: 600;
		cursor: pointer;
	}

	.votes button {
		border: none;
		background-color: #fafafa;
		color: #647380;
	}

	.head {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.card {
		display: flex;
		flex: 0 1 calc(33.33% - 3rem); /* Adjust width to fit 3 cards in a row with spacing */
		margin: 0.5rem 1rem;
		border-radius: 5px;
		background-color: #fafafa;
		align-items: center;
		padding: 1.5rem 0.5rem;
		justify-content: space-around;
	}

	.titleCover {
		display: flex;
		flex-direction: column;
		margin: 0rem 1rem;
		align-items: center;
		position: relative;
	}

	.imageContainer {
		display: flex;
		align-items: center;
	}

	.imageContainer img {
		width: 120px;
		height: 180px;
		border-style: none;
		object-fit: cover;
		border-radius: 6px;
		box-shadow: 2px 2px 6px rgba(0, 0, 0, 0.3);
		transition: transform 0.3s, box-shadow 0.3s;
	}

	.title {
		color: #5c728a;
		padding-top: 16px;
		font-weight: 700;
		font-size: 14px;
		transition: all 0.3s ease-in-out;
	}

	.titleCover:hover .title {
		color: #1faafa;
	}
</style>

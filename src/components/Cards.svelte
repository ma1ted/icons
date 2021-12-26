<script>
	async function getJson() {
		const file = await fetch("/sites.json");
		const json = await file.json();
		return json;
	}
	const promise = getJson();

	const cardClick = (url) => window.location.href = url; 
</script>

<section id="cards">
	{#await promise}
		<p>Loading JSON...</p>
	{:then sites}
		{#each Object.values(sites) as site}
			<a class="card" href={site.url}>
				<h3>{site.name}</h3>
				<h4>{site.desc}</h4>

				<div id="tag-container">
					<span class="tag"><span class="material-icons">gavel</span> {site.licence}</span>
					{#if !site.filters.login}
						<!-- <span class="tag"><span class="material-icons">cloud_off</span> No Login</span> -->
					{/if}
					{#if !site.filters.attribution}
						<span class="tag"><span class="material-icons">article</span> No Attribution</span>
					{/if}
					{#if site.filters.commercial}
						<span class="tag"><span class="material-icons">business</span> Commercial</span>
					{/if}
					<span id="rating-container" class="tag">
						{#each Array.from(Array(site.rating)) as star}
							<span id="star" class="material-icons">star</span>
						{/each}
						{#if site.rating % 1 !== 0}
							<span id="star" class="material-icons">star_half</span>
						{/if}
					</span>
					<span class="tag"><span class="material-icons">numbers</span> {site.count}</span>
				</div>
			</a>
		{/each}
	{:catch error}
		<p class="error">{error.message}</p>
	{/await}
</section>

<style>
	a.card {
		background-color: var(--alt-card);
		border: 2px solid var(--alt-border);
		border-radius: 10px;

		text-decoration: none;

		display: block;
		padding: 1rem;
		margin: 1rem auto;
		max-width: 30rem;

		transition: border ease-out 0.25s,
					transform ease-out 0.1s;
	}
	@media(hover: hover) and (pointer: fine) {
		a.card:hover {
			border: 2px solid var(--alt-red);
			transform: scale(1.1);
		}
	}
	a.card:active {
		transform: scale(0.9);
		border: 2px solid var(--alt-green);
	}

	h3 {
		margin: 0;
		color: white;
	}
	h4 {
		font-size: 0.8rem;
		font-weight: 200;
		color: var(--alt-light);
	}

	#tag-container {
		padding-top: 0.5rem;
		display: flex;
		flex-wrap: wrap;
		gap: 5px;
	}
	.tag {
		padding: 5px;
		border-radius: 50px;
		border: 1px solid var(--alt-tag-border);
		background-color: var(--alt-border);
		color: var(--alt-light);
		font-size: 0.8rem;
	}
	.tag > span {
		vertical-align: text-bottom;
		font-size: inherit;
		padding: 2px;
		color: var(--alt-yellow)
	}
	.tag > #star {
		padding: 1px;
	}

	.error {
		color: red;
	}
</style>

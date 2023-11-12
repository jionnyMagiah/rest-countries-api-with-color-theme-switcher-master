<script>
	import { onMount } from 'svelte';
	import CountryComponent from './CountryComponent.svelte';

	const apiFull = `https://restcountries.com/v3.1/all?fields=cca3,name,flags,population,region,capital`; // api endpoint for data

	let srcText;
	let countries = [];
	let filtered = [];
	async function getCountries(api) {
		console.log(api);
		const response = await fetch(api);
		countries = await response.json();
		countries.forEach((c) => {
			c.show = true;
		});
		filtered = countries;
	}

	onMount(() => {
		getCountries(apiFull);
	});

	function filter_names(country) {
		return country.name.common.toLowerCase().includes(srcText.toLowerCase());
	}
	function filterCountries() {
		filtered = countries.filter(filter_names);
	}

	function filterRegion() {
		let region = this.dataset.value;
		if (region === 'all') {
			getCountries(apiFull);
		} else {
			let apiRegion = `https://restcountries.com/v3.1/region/${region}?fields=cca3,name,flags,population,region,capital`;
			getCountries(apiRegion);
		}
	}
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div class="filter">
	<input
		type="text"
		placeholder="Search Country..."
		bind:value={srcText}
		on:input={filterCountries}
	/>
	<div class="dropdown drop_hist">
		<div class="filter-by">Filter by Region:</div>
		<div class="dropdown-content">
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<!-- svelte-ignore a11y-no-static-element-interactions -->
			<div data-value="all" on:click={filterRegion}>All</div>
			<div data-value="africa" on:click={filterRegion}>Africa</div>
			<div data-value="america" on:click={filterRegion}>America</div>
			<div data-value="asia" on:click={filterRegion}>Asia</div>
			<div data-value="europe" on:click={filterRegion}>Europe</div>
			<div data-value="oceania" on:click={filterRegion}>Oceania</div>
		</div>
	</div>
</div>
<div class="grid-countries">
	{#if filtered.length>0}
		
	{#each filtered as c}
		<div class:show={!c.show}>
			<CountryComponent {...c} />
		</div>
	{/each}
	{:else}
	<div class="no-data">No country found.</div>
	{/if}
</div>

<style>
	.no-data{
		font-size: 20px;
		font-weight: bold;
	}
	input {
		height: 30px;
		margin-bottom: 50px;
		background-color: var(--elements);
		font-size: 16px;
		color: var(--text);
		border-color: rgba(0, 0, 0, 0);
		border-radius: 10px;
	}
	.filter {
		display: flex;
		justify-content: space-between;
		margin-left: 50px;
		margin-right: 50px;
	}
	.show {
		display: none;
	}
	.grid-countries {
		display: grid;
		grid-template-columns: auto auto auto;
		justify-content: space-between;
		margin-left: 50px;
		margin-right: 50px;
		overflow: visible;
	}

	.dropdown {
		position: relative;
		display: inline-block;
	}

	.dropdown-content {
		display: none;
		position: absolute;
		right: -50px;
		background-color: var(--background);
		border-radius: 10px;
		min-width: 160px;
		margin-right: 50px;
	}
	.dropdown-content div {
		color: var(--text);
		border-radius: 10px;
		margin: 12px;
		display: block;
		cursor: pointer;
	}

	.dropdown-content div:hover {
		margin-left: 0.6rem;
	}

	.dropdown:hover .dropdown-content {
		display: block;
	}
</style>

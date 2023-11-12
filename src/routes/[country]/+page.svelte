<script>
	import { onMount } from 'svelte';
	import BorderComponent from './BorderComponent.svelte';
	export let data;
	$: code = data.code;

	let country;
	let status = false;
	let dataCollected = false;
	async function getCountries() {
		const api = `https://restcountries.com/v3.1/alpha/${code}`;
		const response = await fetch(api);
		status = true;
		country = (await response.json())[0];
		dataCollected = country ? true : false;
	}

	$: code, getCountries();

	onMount(() => {
		getCountries();
	});
</script>

<a href="/"><span class="material-symbols-outlined"> arrow_back </span> Back</a>

<div class="view">
	{#if status}
		{#if dataCollected}
			<img src={country.flags.svg} alt={country.flags.alt} srcset="" />
			<div class="info">
				<div class="name">{country.name.common}</div>
				<div class="small-info">
					<div class="left">
						<div>
							<span class="label">Native Name </span>
							{country.name.nativeName[Object.keys(country.name.nativeName)[0]].official}
						</div>
						<div>
							<span class="label">Population </span>{country.population}
						</div>
						<div>
							<span class="label">Region </span>{country.region.replaceAll(',', ', ')}
						</div>
						<div>
							<span class="label">Sub Region </span>{country.subregion.replaceAll(',', ', ')}
						</div>
						<div>
							<span class="label">Capital </span>{country.capital.toString().replaceAll(',', ', ')}
						</div>
					</div>
					<div class="right">
						<div>
							<span class="label">Top Level Domain </span>{country.tld[0].replaceAll(',', ', ')}
						</div>
						<div>
							<span class="label">Currencies </span>
							{country.currencies[Object.keys(country.currencies)[0]].name.replaceAll(',', ', ')}
						</div>
						<div>
							<span class="label">Languages </span>
							{Object.values(country.languages).toString().replaceAll(',', ', ')}
						</div>
					</div>
				</div>

				<div class="border">
					<span class="label">Border countries: </span>
					<div class="borders">
						{#if country.borders}
							{#each country.borders as c}
								<BorderComponent code={c} />
							{/each}
						{:else}
							No countries at the borders.
						{/if}
					</div>
				</div>
			</div>
		{:else}
			<div class="message">Country not found, please check the code.</div>
		{/if}
	{:else}
		<div class="message">I'm collecting the data...</div>
	{/if}
</div>

<style>
	.message {
		font-size: 20px;
		font-weight: bold;
	}
	.name {
		font-weight: bold;
		font-size: 36px;
		margin-bottom: 15px;
	}
	a {
		display: flex;
		justify-content: space-evenly;
		align-items: center;
		margin-left: 50px;
		margin-top: 50px;
		font-size: 18px;
		padding-left: 0;
		padding-right: 0;
		width: 170px;
		height: 40px;
		border-radius: 10px;
		background-color: var(--elements);
		color: var(--text);
		border-color: rgba(0, 0, 0, 0);
	}

	.left,
	.right {
		display: grid;
		height: fit-content;
	}

	.left div,
	.right div {
		margin-bottom: 10px;
	}

	.small-info {
		display: grid;
		grid-template-columns: auto auto;
	}

	.view {
		display: flex;
		margin-right: 50px;
	}

	img {
		max-width: 50vw;
		display: flex;
	}
	.view {
		margin-left: 50px;
		margin-top: 50px;
	}

	.info {
		margin-left: 50px;
		width: 100%;
	}

	.label {
		font-weight: bold;
	}

	.border {
		margin-top: 100px;
		align-items: first baseline;
		display: flex;
	}

	.border .label {
		width: 200px;
	}

	.borders {
		display: grid;
		grid-template-columns: auto auto auto;
		justify-content: space-between;
		width: 100%;
	}

	.right {
		margin-left: 5px;
	}
</style>

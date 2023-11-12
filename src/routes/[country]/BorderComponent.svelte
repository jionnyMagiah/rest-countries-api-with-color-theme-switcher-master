<script>
	import { onMount } from 'svelte';
	export let code;

	let country;
	async function getCountries() {
		const api = `https://restcountries.com/v3.1/alpha/${code}?fields=name`;
		const response = await fetch(api);
		country = await response.json();
	}
	$: code, getCountries();
	onMount(() => {
		console.log(code);
		getCountries();
	});
</script>

{#if country}
	<a href={code}>
		<div class="block">
			{country.name.common}
		</div>
	</a>
{/if}

<style>
	.block {
		display: flex;
		width: 100px;
		justify-content: center;
		text-align: center;
		min-height: 30px;
		background-color: var(--elements);
		margin-left: 10px;
		align-items: center;
		border-radius: 5px;
		margin-bottom: 5px;
	}
</style>

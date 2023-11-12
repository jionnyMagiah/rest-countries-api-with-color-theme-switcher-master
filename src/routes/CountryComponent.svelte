<script>
	export let name = '';
	export let cca3 = '';
	export let flags = '';
	export let population = 0;
	export let region = '';
	export let capital = '';
</script>

<a href="/{cca3}">
    <div class="card">
        <div class="flag">
            <img src={flags.svg} alt="Flag not found" />
        </div>
        <div class="name data">{name.common}</div>
        <div class="small-info">
            <div class="population data"><span class="label">Population: </span>{population}</div>
            <div class="region data"><span class="label">Region: </span>{region}</div>
            <div class="capital data"><span class="label">Capital: </span>{capital}</div>
        </div>
    </div>
</a>

<style>
	img {
        width: 100%;
		border-top-left-radius: 10px;
		border-top-right-radius: 10px;
	}

	.card {
		background-color: var(--elements);
		margin-bottom: 70px;
		border-radius: 10px;
		height: fit-content;
		width: 350px;
	}

	.name {
		font-weight: bold;
		font-size: 24px;
        margin-top: 20px;
	}
	.data {
		margin-left: 20px;
	}
	.label {
		font-weight: bold;
	}
    .small-info{
        margin-top: 20px;
        margin-bottom: 50px;
    }
</style>

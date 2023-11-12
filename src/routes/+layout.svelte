<script>
	import './global.css';

	const dark_mode = 'Dark Mode';
	const light_mode = 'Light Mode';
	let mode = true;

	function toggleMode() {
		mode = !mode;
		document.body.classList.toggle('dark');
	}
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->

<div class="header">
	<div class="title">Where in the world?</div>
	<div class="dark-mode" on:click={toggleMode}>
		<span class="material-symbols-outlined"> {mode ? 'light_mode' : 'dark_mode'} </span>
		{mode ? light_mode : dark_mode}
	</div>
</div>

<div class="main">
	<slot />
</div>

<style>
	.main {
		margin-top: 100px; 
		/* height of the top bar */
	}
	.header {
		display: flex;
		justify-content: space-between;
		background-color: var(--elements);
		font-weight: bold;
		align-items: center;
		padding-left: 50px;
		padding-right: 50px;
		padding-top: 20px;
		padding-bottom: 20px;
		box-shadow: 0 0px 2px 0px var(--input);
		position: fixed; /* Set the navbar to fixed position */
		top: 0; /* Position the navbar at the top of the page */
		width: calc(100% - 100px); /* Full width */
	}
	.dark-mode {
		display: flex;
		align-items: center;
		cursor: pointer;
	}

	.title {
		font-size: 30px;
	}
	.dark-mode span {
		margin-right: 10px;
	}
</style>

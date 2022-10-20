<script lang="ts">
	import Nav from '../components/Nav.svelte'
</script>

<style>
	main {
		position: relative;
		box-sizing: border-box;
		margin: 0 auto;
		height: auto;
		max-width: 1280px;
		min-width: 375px;
		background-color: white;
	}
</style>

<svelte:head>
    <title>Player list UI</title>
</svelte:head>

<Nav />

<main>
	<slot></slot>
</main>

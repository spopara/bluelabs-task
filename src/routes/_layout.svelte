<script lang="ts">
	import Nav from '../components/Nav.svelte'
</script>

<style>
	main {
		position: relative;
		max-width: 1280px;
		min-width: 375px;
		background-color: white;
		margin: 0 auto;
		box-sizing: border-box;
	}
</style>

<svelte:head>
    <title>Player list UI</title>
</svelte:head>

<Nav />

<main>
	<slot></slot>
</main>

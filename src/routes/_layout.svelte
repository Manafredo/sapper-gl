<script lang="ts">
	import Nav from '../components/Nav.svelte';
	import Footer from '../components/Footer.svelte';
	import { stores } from '@sapper/app';
	const {page} = stores();

	export let segment: string;
</script>

<style>
	main {
		position: relative;
		max-width: 56em;
		background-color: white;
		padding: 2em;
		margin: 0 auto;
		box-sizing: border-box;
	}
</style>

{#if $page.path!=="/"}
	<Nav {segment}/>
{/if}


<slot></slot>


<Footer {segment} />
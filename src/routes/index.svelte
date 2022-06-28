<script>
	import { onMount } from 'svelte';

	import Card from '../components/Card.svelte';
	import Accordion from '../components/Accordion.svelte';

	const CHANNEL_ID = 'UCqadGlYJrhCbHzsCnXWOmpA';

	let mySubscriptions = undefined;

	onMount(async () => {
		const response = await fetch(`http://localhost:8080/channel/${CHANNEL_ID}`);

		mySubscriptions = await response.json();
	});
</script>

<main>
	<h1>Subscriptions</h1>

	{#if mySubscriptions === undefined}
		<p>Loading...</p>
	{:else}
		{#each mySubscriptions as { snippet }}
			<article>
				<Card {snippet} />
				<Accordion channelId={snippet.resourceId.channelId} />
			</article>
		{/each}
	{/if}
</main>

<style>
	main {
		max-width: 960px;
		padding: 2rem 1rem;
		margin: 0 auto;
	}

	article {
		border-radius: 1rem;
		box-shadow: 0 0 0.8rem #e8e8e8;
		padding: 1rem;
	}

	article:not(:first-of-type) {
		margin-top: 1rem;
	}
</style>

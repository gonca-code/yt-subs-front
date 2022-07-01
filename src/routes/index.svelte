<script>
	import { onMount } from 'svelte';
	import { io } from 'socket.io-client';

	import Card from '../components/Card.svelte';
	import Accordion from '../components/Accordion.svelte';

	const socket = io('http://localhost:8080');

	const CHANNEL_ID = 'UCqadGlYJrhCbHzsCnXWOmpA';

	let current = 0;
	let total = 0;
	let end = false;
	let mySubscriptions = [];

	onMount(() => {
		socket.on('connect', () => {
			console.log(`Socket connected ${socket.id}`);

			socket.emit('start', { channelId: CHANNEL_ID });
		});

		socket.on('progress', (response) => {
			console.log('🚀 ~ progress response', response);

			current = response.current;
			total = response.total;
			mySubscriptions = [...mySubscriptions, ...response.items];
		});

		socket.on('end', (response) => {
			console.log('🚀 ~ end response', response);

			end = true;
		});
	});
</script>

<main>
	<h1>Subscriptions</h1>

	<p>{current}/{total}</p>
	<p>end: {end ? 'yes' : 'no'}</p>

	{#each mySubscriptions as { snippet }}
		<article>
			<Card {snippet} />
			<Accordion channelId={snippet.resourceId.channelId} />
		</article>
	{/each}
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

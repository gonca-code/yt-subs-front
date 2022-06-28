<script>
	export let channelId;

	let isOpen = false;
	let channelSubscriptions = undefined;

	async function getSubscriptions() {
		const response = await fetch(`http://localhost:8080/channel/${channelId}`);

		channelSubscriptions = await response.json();
	}

	async function handleClick() {
		isOpen = !isOpen;

		if (!channelSubscriptions) {
			await getSubscriptions();
		}
	}
</script>

<button on:click={handleClick}>{isOpen ? 'Hide subscriptions' : 'See subscriptions'}</button>

{#if isOpen}
	{#if channelSubscriptions === undefined}
		<p>Loading...</p>
	{:else}
		<ol>
			{#each channelSubscriptions as { snippet }}
				<li>
					<a target="_blank" href="https://www.youtube.com/channel/{snippet.resourceId.channelId}">
						{snippet.title}
					</a>
				</li>
			{:else}
				<p>Empty</p>
			{/each}
		</ol>
	{/if}
{/if}

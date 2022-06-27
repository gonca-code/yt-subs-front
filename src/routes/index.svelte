<script>
	import ytSubs from '../yt-subs.json';

	const content = [...ytSubs].sort((a, b) => a.snippet.title.localeCompare(b.snippet.title));
</script>

<ul>
	{#each content as { snippet }, index}
		<li>
			<a target="_blank" href="https://www.youtube.com/channel/{snippet.resourceId.channelId}">
				{index + 1}: {snippet.title}
			</a>
		</li>
	{/each}
</ul>

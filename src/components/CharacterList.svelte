<script>
	import { createEventDispatcher } from "svelte";
	export let characters;
	export let isLoading;

	const dispatch = createEventDispatcher();

	const toggleStatus = c => {
		dispatch("togglestatus", c);
	};
</script>

<div id="character-list">
	{#if characters.length > 0}
		<div class="character table-title">
			<span class="character-name">NAME</span>
			<span class="character-status">STATUS</span>
		</div>
		{#each characters as character}
			<div class="character table-row" on:click={() => toggleStatus(character)}>
				<span class="character-name">{character.name}</span>
				<span class="character-status">{character.status}</span>
			</div>
		{/each}
	{/if}
	{#if isLoading}
		<div class="row">Loading...</div>
	{/if}
</div>

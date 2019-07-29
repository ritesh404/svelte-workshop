<script>
	import { createEventDispatcher } from "svelte";

	const dispatch = createEventDispatcher();

	const onTermChange = e => {
		dispatch("termchange", e.target.value);
	};
</script>

<div class="row search-box">
	<input
		type="text"
		id="search-input"
		on:keyup={onTermChange}
		placeholder="Enter Character Name" />
	<button id="search-button" on:click={() => dispatch('search')}>Search</button>
</div>

<script>
	import jq from "jquery";

	const API = Object.freeze({
		ALL: "https://swapi.co/api/people/",
		SEARCH: "https://swapi.co/api/people/?search="
	});

	// APP STATE
	let characters = [];
	let searchTerm = "";
	let isLoading = false;

	async function fetchAllCharacters() {
		isLoading = true;
		try {
			const response = await fetch(API.ALL, {
				method: "GET",
				mode: "cors"
			});
			const result = await response.json();
			isLoading = false;
			return result.results.map(c => ({ status: "Alive", ...c }));
		} catch (e) {
			isLoading = false;
			return [];
		}
	}

	async function fetchCharacter(term) {
		isLoading = true;
		try {
			const response = await fetch(`${API.SEARCH}${term}`, {
				method: "GET",
				mode: "cors"
			});
			const result = await response.json();
			isLoading = false;
			return result.results;
		} catch (e) {
			isLoading = false;
			return [];
		}
	}

	async function handleClick() {
		searchTerm === ""
			? (characters = await fetchAllCharacters())
			: (characters = await fetchCharacter(searchTerm));
	}

	function toggleStatus(character) {
		characters = characters.map(function(c) {
			console.log("here");
			if (c.name === character.name) {
				const newStatus = c.status === "Alive" ? "Dead" : "Alive";
				return { ...c, status: newStatus };
			}
			return c;
		});
	}
</script>

<header class="container">
	<div class="row title">
		<h1>God Of Star Wars!</h1>
	</div>
</header>
<section class="container">
	<div class="row search-box">
		<input
			type="text"
			id="search-input"
			bind:value={searchTerm}
			placeholder="Enter Character Name" />
		<button id="search-button" on:click={handleClick}>Search</button>
	</div>
</section>
<section class="container">
	<div id="character-list">
		{#if characters.length > 0}
			<div class="character table-title">
				<span class="character-name">NAME</span>
				<span class="character-status">STATUS</span>
			</div>
			{#each characters as character}
				<div
					class="character table-row"
					on:click={() => toggleStatus(character)}>
					<span class="character-name">{character.name}</span>
					<span class="character-status">{character.status}</span>
				</div>
			{/each}
		{/if}
		{#if isLoading}
			<div class="row">Loading...</div>
		{/if}
	</div>
</section>

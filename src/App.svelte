<script>
	import jq from "jquery";
	import Section from "./components/Section.svelte";
	import SearchBox from "./components/SearchBox.svelte";
	import Header from "./components/Header.svelte";
	import CharacterList from "./components/CharacterList.svelte";

	const API = Object.freeze({
		ALL: "https://swapi.co/api/people/",
		SEARCH: "https://swapi.co/api/people/?search="
	});

	// APP STATE
	let characters = [];
	let searchTerm = "";
	let isLoading = false;

	const transformResponse = c => ({ status: "Alive", ...c });

	async function fetchAllCharacters() {
		isLoading = true;
		try {
			const response = await fetch(API.ALL, {
				method: "GET",
				mode: "cors"
			});
			const result = await response.json();
			isLoading = false;
			return result.results.map(transformResponse);
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
			return result.results.map(transformResponse);
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

	function toggleStatus(e) {
		const character = e.detail;
		characters = characters.map(function(c) {
			if (c.name === character.name) {
				const newStatus = c.status === "Alive" ? "Dead" : "Alive";
				return { ...c, status: newStatus };
			}
			return c;
		});
	}
</script>

<Header />
<Section>
	<SearchBox
		on:termchange={e => {
			searchTerm = e.detail;
		}}
		on:search={handleClick} />
</Section>
<Section>
	<CharacterList {characters} {isLoading} on:togglestatus={toggleStatus} />
</Section>

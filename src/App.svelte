<script>
	import jq from "jquery";

	const API = Object.freeze({
		ALL: "https://swapi.co/api/people/",
		SEARCH: "https://swapi.co/api/people/?search="
	});

	// APP STATE
	window.characters = [];

	const transformResponse = c => ({ status: "Alive", ...c });

	async function fetchAllCharacters() {
		try {
			const response = await fetch(API.ALL, {
				method: "GET",
				mode: "cors"
			});
			const result = await response.json();
			return result.results.map(transformResponse);
		} catch (e) {
			return [];
		}
	}

	async function fetchCharacter(term) {
		try {
			const response = await fetch(`${API.SEARCH}${term}`, {
				method: "GET",
				mode: "cors"
			});
			const result = await response.json();
			return result.results.map(transformResponse);
		} catch (e) {
			return [];
		}
	}

	function renderCharacters() {
		const characterDivs = characters.map(c => domForCharacter(c));
		const finalList = [title(), ...characterDivs];
		const container = document.getElementById("character-list");
		container.innerHTML = "";

		finalList.map(d => container.appendChild(d));
	}

	function title() {
		const div = domForCharacter({ name: "NAME" }, "STATUS");
		div.setAttribute("class", "character table-title");
		return div;
	}

	function domForCharacter(chara, alive = "Alive") {
		const div = document.createElement("div");
		div.setAttribute("class", "character table-row");
		const name = document.createElement("span");
		name.setAttribute("class", "character-name");
		name.innerText = chara.name;
		const status = document.createElement("span");
		status.setAttribute("class", "character-status");
		status.innerText = alive;
		div.appendChild(name);
		div.appendChild(status);
		div.onclick = function() {
			console.log("here");
			this.setAttribute("class", "character table-row dead");
			const status = jq(this)
				.find(".character-status")
				.text("Dead");
			// status.innerText = "DEAD";
		};
		return div;
	}

	async function handleClick() {
		const searchTerm = jq("#search-input").val();
		if (searchTerm === "") {
			const results = await fetchAllCharacters();
			window.characters = results;
		} else {
			const results = await fetchCharacter(searchTerm);
			window.characters = results;
		}

		renderCharacters();
	}

	jq(document).ready(function() {
		jq("#search-button").on("click", handleClick);
	});
</script>

<header class="container">
	<div class="row title">
		<h1>God Of Star Wars!</h1>
	</div>
</header>
<section class="container">
	<div class="row search-box">
		<input
			autofocus
			type="text"
			id="search-input"
			value=""
			placeholder="Enter Character Name" />
		<button id="search-button">Search</button>
	</div>
</section>
<section class="container">
	<div id="character-list" />
</section>

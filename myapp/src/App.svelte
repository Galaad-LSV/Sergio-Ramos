<script>
  let limit = 20;
  let page = 1;
  let offset = 0;
  $: {offset = (page - 1) * limit;}

  async function getPokedexes() {
    const response = await fetch(`https://pokeapi.co/api/v2/pokemon?limit=${limit}&offset=${offset}`);
    const data = await response.json();
    pokemonList = data.results;
  }
  
  let pokemonList = [];

</script>

<main>
  <h1>Acces à l'API Pokemon</h1>

  <div>
    <label for="limit">
      Nombre de pokemons à afficher
      <input id="limit" name="limit" type="number" min="5" max="20" bind:value={limit}>
    </label>

    <label for="offset">
      Page
      <input id="offset" name="offset" type="number" min="1" max="50" bind:value={page}>
    </label>

    <button id="submit" type="button" on:click={getPokedexes}>Ok</button>
  </div>

  {#await getPokedexes()}
    <p>Chargement...</p>
  {:then}
    <ul>
      {#each pokemonList as pokemon (pokemon.name)}
        <li>{pokemon.name}</li>
      {/each}
    </ul>
  {:catch error}
    <p>Une erreur est survenue</p>
    <p>{error}</p>
  {/await}

</main>

<style>
  li {
    list-style: none;
  }
</style>

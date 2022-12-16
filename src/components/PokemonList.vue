<!-- PAUL DEBRIL-->
<template>
  <!-- The template contains the HTML structure and layout of the component -->
  <div>
    <!-- This div contains the search bar -->
    <div class="searchbar">
      <!-- The input field is bound to the "search" data property, so when the user types something in the field, the "search" property is updated -->
      <input type="text" v-model="search" placeholder="Rechercher un Pokémon" />
    </div>
    <!-- This div contains the list of Pokemon -->
    <div class="list">
      <!-- The v-for directive is used to iterate over the "filteredPokemon" computed property, which contains an array of Pokemon objects. For each iteration, a new article element is created and the Pokemon object is passed to the "pokemon" variable -->
      <article v-for="pokemon in filteredPokemon" :key="pokemon.name">
        <!-- The h3 element displays the name of the Pokemon -->
        <h3>{{pokemon.name}}</h3>
        <!-- The img element displays the Pokemon's image and has a click event listener that calls the "showDetails" method and passes the Pokemon object as an argument -->
        <img v-bind:src="getPokemonImage(pokemon.name)" @click="showDetails(pokemon)" />
        <!-- The v-if directive is used to conditionally render the div element, which displays the details of the Pokemon. It checks if the "selectedPokemon" data property is equal to the current Pokemon object. If it is, the div element is rendered, otherwise it is not -->
        <div v-if="selectedPokemon === pokemon">
          <!-- The p elements display the name and height of the Pokemon -->
          <p>Nom : {{pokemon.name}}</p>
          <p>Poids : {{pokemon.height}}</p>
        </div>
      </article>
    </div>
  </div>
</template>
<script>
import axios from 'axios';

export default {
  // The data object contains the component's data properties
  data() {
    return {
      search: '', // The search field input
      searchResults: [], // The array of Pokemon objects retrieved from the API
      selectedPokemon: null, // The selected Pokemon object
      selectedPokemonDetails: null, // The details of the selected Pokemon object, retrieved from the API
    };
  },
  mounted() {
    // A GET request is made to the PokeAPI to retrieve a list of Pokemon
    axios
      .get('https://pokeapi.co/api/v2/pokemon?limit=201')
      .then((response) => {
        // The response data is stored in the "searchResults" data property
        this.searchResults = response.data.results;
      });
  },
  // Computed properties are functions that return a value that is derived from the component's data
  computed: {
    filteredPokemon() {
      return this.searchResults.filter((pokemon) => {
        return pokemon.name.toLowerCase().includes(this.search.toLowerCase());
      });
    },
  },
  methods: {
    showDetails(pokemon) {
      this.selectedPokemon = pokemon;
      axios
        .get(pokemon.url)
        .then((response) => {
          this.selectedPokemonDetails = response.data;
          console.log(this.selectedPokemon.name)
          console.log(this.selectedPokemonDetails.height)
        });
    },
    getPokemonImage(pokemonName) {
      return `https://img.pokemondb.net/sprites/bank/normal/${pokemonName}.png`;
    },
  },
};
</script>


<style lang="scss" scoped>
.list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  grid-gap: 10px;
  width: 100%;
  max-width: 510px;
}
article {
  height: 150px;
  background-color: #efefef;
  text-align: center;
  text-transform: capitalize;
  border-radius: 5px;
  cursor: pointer;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
}
h3 {
  margin: 0;
}
#scroll-trigger {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 150px;
  font-size: 2rem;
  color: #efefef;
}

img {
  width: 96px;
  height: 96px;
}




.searchbar {
  position: relative;
  width: 100%;
  max-width: 510px;
  padding-bottom: 20px;
}
input {
  border: none;
  outline: none;
  border-radius: 5px;
  padding: 10px 40px 10px 10px;
  width: calc(100% - 50px);
  font-size: 1rem;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
}
</style>


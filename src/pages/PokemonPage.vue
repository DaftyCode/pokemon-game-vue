<template>
  <h1 v-if="!pokemon">Espere por favor...</h1>
  <div v-else>
    <h1>¿Quién es este Pokemón?</h1>

    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />

    <PokemonOptions :pokemons="pokemonsArr" @selection="checkAnswer($event)" />

    <template v-if="showAnswer">
      <h2>{{ message }}</h2>
      <button @click="newGame">Nuevo Juego</button>
    </template>
  </div>
</template>

<script>
  import PokemonPicture from '@/components/PokemonPicture.vue';
  import PokemonOptions from '@/components/PokemonOptions.vue';
  import getPokemonOptions from '@/helpers/getPokemonOptions';

  export default {
    components: { PokemonOptions, PokemonPicture },
    data() {
      return {
        pokemonsArr: [],
        pokemon: null,
        showPokemon: false,
        showAnswer: false,
        message: '',
      };
    },
    methods: {
      async mixPokemonsArray() {
        this.pokemonsArr = await getPokemonOptions();

        const rndInt = Math.floor(Math.random() * 4);
        this.pokemon = this.pokemonsArr[rndInt];
      },
      checkAnswer(pokemonId) {
        this.showPokemon = true;
        this.showAnswer = true;

        if (pokemonId === this.pokemon.id) {
          this.message = `Correcto!!!, El pokemón es ${this.pokemon.name}`;
        } else {
          this.message = `Oooopppss, El pokemón era ${this.pokemon.name}`;
        }
      },
      newGame() {
        this.showPokemon = false;
        this.showAnswer = false;
        this.pokemonsArr = [];
        this.pokemon = null;
        this.message = '';
        this.mixPokemonsArray();
      },
    },
    mounted() {
      this.mixPokemonsArray();
    },
  };
</script>

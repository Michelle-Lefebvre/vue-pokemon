<template>
  <div class="w-gull flex justify-center">
    <input
      type="text"
      placeholder="Enter Pokemon here"
      class="nt-10 p-2 border-blue-500 border-2"
      v-model="text"
    />
  </div>
  <div class="mt-10 p-4 flex flex-wrap justify-center">
    <div
      class="ml-4 text-2x text-blue-400"
      v-for="(pokemon, idx) in filteredPokemon"
      :key="idx"
    >
      {{ pokemon.name }}
    </div>
  </div>
</template>

<script>
// @ is an alias to /src toRefs allows us to take objects and destructure them while still being active
import { reactive, toRefs, computed } from 'vue';
export default {
  name: 'Home',
  setup() {
    const state = reactive({
      pokemons: [],
      urlIdLookuup: {},
      text: '',
      filteredPokemon: computed(() => updatePokemon())
    });

    function updatePokemon() {
      if (!state.text) {
        return [];
      }
      return state.pokemons.filter((pokemon) =>
        pokemon.name.includes(state.text)
      );
    }

    fetch('https://pokeapi.co/api/v2/pokemon?offset=0')
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        state.pokemons = data.results;
        (state.urlIdLookuup = data.results.reduce(
          (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 })
        )),
          {};
      });
    return { ...toRefs(state) };
  }
};
</script>

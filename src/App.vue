<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Header 
    v-bind:types="types" 
    v-bind:filter="filter" 
    v-bind:sort="sort"
    />
    <Results v-bind:sortedList="sortedList"/>
  </div>
</template>

<script>
import pokedex from '../pokedex.js';
import Header from './components/Header.vue';
import Results from './components/Results.vue';

export default {
  name: 'app',
  data(){
    return {
      pokedex,
      filter: {
        type: 'all',
        attack: ''
      },
      sort: {
        sorted: 'id'
      }
    };
  },
  components: {
    Header,
    Results
  },
  computed: {
    filtered() {
      const { type, attack } = this.filter;
      const typeFiltered = this.pokedex.slice().filter(p => type === 'all' || p.type_1 === type || p.type_2 === type);
      const attackFiltered = typeFiltered.filter(pokemon => pokemon.attack > attack);
      return attackFiltered;
    },

    sortedList() {
      const { sorted } = this.sort;
      return this.filtered.slice().sort((a, b) => {
        const sortedA = a[sorted];
        const sortedB = b[sorted];
        if(sortedA > sortedB) return 1;
        if(sortedA < sortedB) return -1;
        return 0;
      });
    },

    types() {
      const typeOne = pokedex.map(pokemon => pokemon.type_1);
      const typeTwo = pokedex.map(pokemon => pokemon.type_2);
      const allTypes = new Set (typeOne.concat(typeTwo));

      return [...allTypes.values()];
    }
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

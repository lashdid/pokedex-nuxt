<template>
  <section
    class="flex flex-col items-center max-w-3xl mx-auto my-10 p-3 space-y-5 shadow rounded"
  >
    <h1 class="text-3xl font-bold">PokéDex</h1>
    <SearchBar @on-type="onSearch" />
    <List :items="items" />
    <PaginationBar
      :prevButton="prevUrl"
      :nextButton="nextUrl"
      @click-prev="getItemsByPage('prev')"
      @click-next="getItemsByPage('next')"
    />
  </section>
</template>

<script lang="ts">
import Vue from "vue";
let baseUrl = "https://pokeapi.co/api/v2/pokemon";

export default Vue.extend({
  head: {
    title: "Nuxt Pokedex",
  },
  async created() {
    this.firstItems = await this.getItems(baseUrl);
    this.getAllItems();
  },
  data() {
    let nextUrl = "";
    let prevUrl = "";
    interface ItemProps {
      image: string;
      name: string;
      order: number;
    }
    let items: ItemProps[] = [];
    let fullItems: ItemProps[] = [];
    let firstItems: ItemProps[] = [];
    return { items, fullItems, firstItems, nextUrl, prevUrl };
  },
  methods: {
    onSearch(text: string) {
      if (text) {
        const data = JSON.parse(JSON.stringify(this.fullItems));
        this.items = data.filter((e: { name: string }) =>
          e.name.includes(text.toLowerCase())
        );
        this.prevUrl = "";
        this.nextUrl = "";
      } else {
        this.items = this.firstItems;
        this.nextUrl = "https://pokeapi.co/api/v2/pokemon?offset=20&limit=20";
      }
    },
    async fetchApi(url: string) {
      const api = await fetch(url);
      return await api.json();
    },
    async mapItems(pokemons: []) {
      return pokemons.map(async (pokemon: { name: string; url: string }) => {
        const pokemonApi = await fetch(pokemon.url);
        const pokemonRes = await pokemonApi.json();
        return {
          image: pokemonRes.sprites.front_default,
          order: pokemonRes.order,
          name: pokemonRes.name,
        };
      });
    },
    async getItems(url: string) {
      const res = await this.fetchApi(url);
      this.nextUrl = res.next;
      this.prevUrl = res.previous;
      const pokemons = await this.mapItems(res.results);
      this.items = await Promise.all([...pokemons]);
      return this.items;
    },
    async getAllItems() {
      let url = "https://pokeapi.co/api/v2/pokemon?limit=1000";
      const res = await this.fetchApi(url);
      const pokemons = await this.mapItems(res.results);
      this.fullItems = await Promise.all([...pokemons]);
    },
    async getItemsByPage(page: string) {
      if (page === "next") {
        this.getItems(this.nextUrl);
        baseUrl = this.nextUrl;
      } else {
        this.getItems(this.prevUrl);
        baseUrl = this.prevUrl;
      }
    },
  },
});
</script>

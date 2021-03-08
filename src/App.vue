<template>
  <form @submit.prevent="searchItunes(searchText)">
    <input type="text" v-model="searchText" />
    <button @click="searchItunes(searchText)">Search</button>
    <div v-if="data.results">
      <div v-for="album in data.results" :key="album.artistId">
        <!-- <h2>Album Name: {{ album.collectionName }}</h3>
        <h4>Artwork</h5>
        <img :src="album.artworkUrl99" alt="" />
        <h4>Price: {{ album.collectionPrice }}</h4> -->
        <TheShowAlbum :album="album" />
      </div>
    </div>
  </form>
</template>

<script lang="ts">
import { defineComponent, reactive, ref, toRefs } from "vue";
import { itunesSearch } from "./services/iTunesAPI";
import { ItunesTypes } from "./types/ItunesTypes.interface";
import TheShowAlbum from "./components/TheShowAlbum.vue";

export default defineComponent({
  name: "App",
  components: {
    TheShowAlbum
  },
  // data: () => {
  //   return {
  //     data: {} as ItunesTypes,
  //     searchText: ""
  //   };
  // },
  // methods: {
  //   async searchItunes(search: string): Promise<void> {
  //     const value = await itunesSearch(search);
  //     this.data = value;
  //     console.log("data", value);
  //   }
  // },
  setup() {
    let albums = reactive<{ data: ItunesTypes }>({ data: {} });
    let searchText = ref("");
    const searchItunes = async (search: string): Promise<void> => {
      const value = await itunesSearch(search);
      albums.data = value;
      console.log("data", albums);
    };

    return { searchItunes, ...toRefs(albums), searchText };
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

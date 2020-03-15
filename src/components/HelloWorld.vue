<template>
  <div class="container max-w-3xl mx-auto">
    <h1 class="text-xl font-bold text-center mt-4">¡¡Bienvenid@!!</h1>
    <p
      class="text-center"
    >A continuación tienes un montón de razas de perretes. Clickea en ellas para poder ver fotos de cada una de las razas y que sea más amena esta cuarentena :)</p>

    <div class=" mx-auto mt-4">
      <input
        v-model="searchByBreed"
        class="shadow appearance-none border rounded w-64 py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline mb-4"
        id="breed"
        type="text"
        placeholder="Busca tu raza favorita!"
      />

      <ul class>
        <li class="cursor-pointer" v-for="breed in filteredByBreed" :key="breed.id">
          <div class="text-indigo-700" @click="getImagesByBreed(breed)">-{{ breed }}</div>
          <div @click="getSubBreeds()"></div>
          <div class="flex flex-wrap" v-if="selectedBreed === breed">
            <div v-for="(image, index) in imagesByBreed" :key="image.id">
              <img v-if="index < 10" class="h-24 m-2" :src="image" :alt="index" />
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      breeds: {},
      searchByBreed: "",
      selectedBreed: [],
      imagesByBreed: []
    };
  },
  computed: {
    filteredByBreed() {
      return Object.keys(this.breeds).filter(breed =>
        breed.toLowerCase().includes(this.searchByBreed.toLowerCase())
      );
    },

  },
  methods: {
    getBreeds() {
      axios
        .get("https://dog.ceo/api/breeds/list/all")
        .then(response => (this.breeds = response.data.message));
    },
    getImagesByBreed(breed) {
      this.selectedBreed = breed;
      axios
        .get("https://dog.ceo/api/breed/" + breed + "/images")
        .then(response => (this.imagesByBreed = response.data.message));
    }
  },
  created() {
    this.getBreeds();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
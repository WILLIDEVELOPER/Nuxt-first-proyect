<template>
  <div class="bg-slate-700  col-span-1 flex flex-col items-center">
    <div class="flex items-center p-[1rem]  gap-[1rem] ">
      <div class="flex flex-col gap-[0.5rem]">
        <img :src="imagePoke" alt="" class="w-[10rem]" />
        <p class="text-center text-white font-bold">{{ nombre }}</p>
      </div>
      <div class="text-white ">
        <div>
          <h1>Estadisticas</h1>
          <label> </label>
        </div>
        <div>

        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "PokemonPage",
  data() {
    return {
      nombre: $nuxt.$route.params.pokemon,
      imagePoke: $nuxt.$route.query.image,
      pokemonImages: {},
    };
  },
  methods: {
    async getPokemon() {
      try {
        const { data: res } = await this.$axios.get(
          `https://pokeapi.co/api/v2/pokemon/${this.nombre}`
        );

        // Filtrar los elementos nulos de res.sprites
        this.pokemonImages.imagenDefecto = Object.values(res.sprites).filter(
          (item) => item !== null && typeof item == "string"
        );
        console.log(this.pokemonImages.imagenDefecto);
        // this.pokemonImages.otherImages
      } catch (error) {
        console.log(error);
      }
    },
  },
  async mounted() {
    await this.getPokemon();
  },
};
</script>
<style></style>

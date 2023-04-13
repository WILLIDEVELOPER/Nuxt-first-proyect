<template>
  <div>
    <div v-if="loading" class="h-screen  flex justify-center items-center">
      <Load />
    </div>
    <div v-else class="flex flex-col gap-[1rem] h-full bg-slate-700 p-[1rem]">
      <h1
        class="bg-slate-300 font-bold text-center text-4xl rounded-lg p-[0.3rem]"
      >
        PokeDEx
      </h1>
      <div class="text-white ">
        <input
        class="self-start rounded-md p-[0.3rem] text-black font-bold outline-none"
        type="text"
        v-model="search"
        placeholder="Buscar Pokemon"
        @change="cambiar"
      />
      <label class="p-[1rem]">{{mensaje}}</label>
      </div>
      <div id="cards" class="grid grid-cols-4 gap-[1rem]">
        <Card
          :key="index"
          v-for="(item, index) in pokemons"
          :nombre="item.name"
          :link="item.url"
        />
      </div>

      <div class="w-full flex justify-center gap-[1rem]">
        <button
          class="bg-black text-white font-bold p-[0.3rem] rounded-md"
          :hidden="show"
          @click="paginadoPrev"
        >
          Anterior
        </button>
        <button
          class="bg-black text-white font-bold p-[0.3rem] rounded-md"
          @click="paginadoNext"
        >
          Siguiente
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      enlace: "https://pokeapi.co/api/v2/pokemon",
      image: "",
      pokemons: [],
      n: "",
      prev: "",
      show: true,
      response: {},
      search: "",
      loading: true,
      mensaje: ""
    };
  },
  methods: {
    cambiar() {
      let pokemonFound = this.pokemons.find(el => el.name == this.search)
      if (pokemonFound) {
        this.$router.push({ path: pokemonFound.name, query: { image: pokemonFound.url } });
      }else{
        this.search = ""
        this.mensaje = "Pokemon no encontrado"
        setTimeout(() => {
          this.mensaje = ""
        }, 2000);
      }
    },
    async fetch() {
      try {
        let respuesta;
        const { data: res } = await this.$axios.get(`${this.enlace}`);
        this.response = res;
        this.pokemons = res.results;

        if (this.response.previous != null) {
          this.show = false;
        } else {
          this.show = true;
        }

        for (let i = 0; i < this.pokemons.length; i++) {
          respuesta = await this.getPokemon(this.pokemons[i].name);
          this.image = respuesta.sprites.other.home.front_default;
          this.pokemons[i].url = this.image;
        }


      } catch (error) {
        console.log(error);
      }
    },

    async getPokemon(name) {
      try {
        const { data: res } = await this.$axios.get(
          `https://pokeapi.co/api/v2/pokemon/${name}`
        );
        return res;
      } catch (error) {
        console.log(error);
      }
    },

    async paginadoNext() {
      this.enlace = this.response.next;

      await this.fetch();
    },

    async paginadoPrev() {
      this.enlace = this.response.previous;

      await this.fetch();
    },
  },
  async mounted() {
    await this.fetch();
    this.loading = false
  },
};
</script>

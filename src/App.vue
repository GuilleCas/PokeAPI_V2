<template>
  <div id="PokeAPI-v2">
    <!-- Se define el Background Image -->
    <div
      class="
        min-h-screen
        w-full
        h-full
        bg-no-repeat bg-cover
        items-center
        justify-center
      "
      style="
        background-image: url('https://images.alphacoders.com/113/thumbbig-1130469.webp');
      "
    >
      <!-- Se define el titulo de la pagina -->
      <div class="flex justify-center align-middle flex-row">
        <div class="m-auto">
          <button
            v-on:click="Previous"
            class="
              bg-gray-300
              hover:bg-gray-400
              text-gray-800
              font-bold
              py-3
              px-5
              rounded-l-lg
            "
          >
            Prev
          </button>
        </div>
        <div class="m-auto">
          <a href="">
            <img
              class="mx-auto img-size-per"
              src="https://images.wikidexcdn.net/mwuploads/esssbwiki/7/77/latest/20111028181540/TituloUniversoPok%C3%A9mon.png"
              alt="Pokedex"
            />
          </a>
        </div>
        <div class="m-auto">
          <button
            v-on:click="Next"
            class="
              bg-gray-300
              hover:bg-gray-400
              text-gray-800
              font-bold
              py-3
              px-5
              rounded-r-lg
            "
          >
            Next
          </button>
        </div>
      </div>
      <!-- Contenido de la Pokedex -->
      <div class="justify-items-center grid - grid-cols-5 gap-4">
        <div
          v-for="(data, index) in pokemons"
          :key="index"
          class="max-w-xs h-52 rounded border-2 border-gray-900 overflow-hidden"
        >
          <img :src="data.img" class="w-full h-28" />
          <div class="bg-gray-100 px-6 py-4">
            <div class="font-bold text-base mb-0">{{ data.nombre }}</div>
          </div>
          <div class="bg-gray-200 px-4 pt-2 pb-0.5">
            <span
              class="
                inline-block
                bg-gray-300
                rounded-full
                px-2
                py-1
                text-xs
                font-semibold
                text-gray-700
                mr-1
                mb-1
              "
              >{{ data.habilidad1 }}</span
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "PokeAPI-v2",
  data() {
    return {
      url: `https://pokeapi.co/api/v2/pokemon/?offset=0&limit=20`,
      next: null,
      back: null,
      pokemons: [],
      Poke_array: [],
    };
  },
  mounted() {
    axios.get(this.url).then((Response) => {
      //Variable del botón siguiente

      const Next = Response.data.next;
      this.next = Next;

      //Variable del resultados de los pokemons
      const url = Response.data.results;
      this.Poke_array = url;

      //Variable del botón anterior
      const Prev = Response.data.previous;
      this.prev = Prev;

      for (let i = 0; i < 20; i++) {
        axios.get(`${this.Poke_array[i].url}`).then((Response) => {
          let Pokemon = {
            img: Response.data.sprites.front_default,
            nombre: Response.data.name,
            habilidad1: Response.data.abilities[0].ability.name,
            id: Response.data.id,
          };
          this.pokemons.push(Pokemon);
        });
      }
    });
  },
  methods: {
    Previous: function () {
      if (this.prev == null) {
      } else {
        this.Poke_array = [];
        this.pokemons = [];
        axios.get(this.prev).then((Response) => {
          const Next = Response.data.next;
          this.next = Next;
          const Prev = Response.data.previous;
          this.prev = Prev;
          const url = Response.data.results;
          this.Poke_array = url;

          for (let i = 0; i < 20; i++) {
            axios.get(`${this.Poke_array[i].url}`).then((Response) => {
              let Pokemon = {
                img: Response.data.sprites.front_default,
                nombre: Response.data.name,
                habilidad1: Response.data.abilities[0].ability.name,
                id: Response.data.id,
              };
              this.pokemons.push(Pokemon);
            });
          }
        });
      }
    },
    Next : function () {
      this.Poke_array = [];
      this.pokemons = [];
      axios.get(this.next).then((Response) => {
        const Next = Response.data.next;
        this.next = Next;
        const Prev = Response.data.previous;
        this.prev = Prev;
        const url = Response.data.results;
        this.Poke_array = url;

        for(let i = 0; i < 20; i++){
          axios.get(`${this.Poke_array[i].url}`).then((Response) => {
            let Pokemon = {
              img: Response.data.sprites.front_default,
              nombre: Response.data.name,
              habilidad1: Response.data.abilities[0].ability.name,
              id: Response.data.id,
            };
            this.pokemons.push(Pokemon);
          });
        }
      });
    }
  },
};
</script>

<style>
.img-size-per {
  height: 110px;
  width: 330px;
}
</style>

<script setup>
import Type from "./Type.vue";
import PokemonCard from "./PokemonCard.vue";
</script>

<script>
export default {
  props: ["apiUrl"],
  data: () => {
    return {
      pokemons: [],
      nextUrl: "",
      currentUrl: "",
      direction: null,
      startY: 0,
    };
  },
  methods: {
    // scrolled() {
    //   let scrollhead = document.getElementById("top-header");
    //   scrollhead.classList.add("isHidden");
    // J'ai abandonné cette feature par manque de temps, elle fonctionne mais ne détecte pas le sens du scroll
    // },
    fetchData() {
      let req = new Request(this.currentUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) return resp.json();
        })
        .then((data) => {
          this.nextUrl = data.next;
          data.results.forEach((pokemon) => {
            pokemon.id = pokemon.url
              .split("/")
              .filter(function (part) {
                return !!part;
              })
              .pop();
            this.pokemons.push(pokemon);
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
    scrollTrigger() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.intersectionRatio > 0 && this.nextUrl) {
            this.next();
          }
        });
      });

      observer.observe(this.$refs.infinitescrolltrigger);
    },
    next() {
      this.currentUrl = this.nextUrl;
      this.fetchData();
    },
  },

  created() {
    this.currentUrl = this.apiUrl;
    this.fetchData();
  },
  mounted() {
    this.scrollTrigger();
  },
};
</script>

<template>
  <div class="box dex">
    <div class="wrapper list-wrapper">
      <div class="list" id="list">
        <router-link
          v-for="(pokemon, index) in pokemons"
          :key="'poke' + index"
          :to="{
            name: 'pokemon',
            params: { pokemonName: pokemon.name },
          }"
          class="pokemon-card wrapper"
        >
          <div class="infos">
            <p class="pokemon-number">
              #{{
                pokemon.id >= 100
                  ? pokemon.id
                  : pokemon.id >= 10
                  ? "0" + pokemon.id
                  : "00" + pokemon.id
              }}
            </p>
            <h3 class="pokemon-name">{{ pokemon.name }}</h3>
          </div>

          <img
            :src="
              'https://assets.pokemon.com/assets/cms2/img/pokedex/full/' +
              [
                pokemon.id >= 100
                  ? pokemon.id
                  : pokemon.id >= 10
                  ? '0' + pokemon.id
                  : '00' + pokemon.id,
              ] +
              '.png'
            "
            class="sprite"
          />
          <img class="ball" src="../assets/svg/ball.svg" alt="" />
          <img class="dots" src="../assets/svg/dots-2.svg" alt="" />
        </router-link>
        <div id="scroll-trigger" ref="infinitescrolltrigger">
          <i class="fas fa-spinner fa-spin"></i>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.pokemon-card.wrapper {
  background: #8fb3b3;
}

.list-wrapper {
  //   margin-top: 50vh;

  display: flex;
  flex-direction: column;
  height: 100%;
  .list {
    padding-top: 27vh;
    flex-grow: 1;
    height: 100%;

    overflow: scroll;
  }
}

.box.dex {
  //   padding-top: 22px;
  height: 100%;
  button {
    transition: all 0.3s;
    background-color: #5d5e7b;
    padding: 20px;
    width: 57px;
    height: 57px;
    border-radius: 10px;
    border: none;
    color: white;
    &:hover {
      background-color: #64668a;
    }
  }
  .list {
    padding-bottom: 50px;
    height: 100%;
  }

  #scroll-trigger {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 150px;
    font-size: 2rem;
    color: rgb(8, 0, 77);
  }
}

@media screen and(max-width: 580px) {
  .box.dex {
    .list {
      padding-top: 30vh;
    }
  }
}

@media screen and(max-width: 360px) {
  .box.dex {
    .list {
      padding-top: 33vh;
    }
  }
}
</style>

<script setup>
import Type from "../components/Type.vue";
</script>

<script>
export default {
  name: "Pokemon",
  props: ["pokemonUrl", "pokemonName"],
  data: () => {
    return {
      pokemon: {},
      pokemonSpecie: {},
      selected: 0,
      shiny: false,
    };
  },
  methods: {
    fetchData() {
      let req = new Request(`https://pokeapi.co/api/v2/pokemon/${this.pokemonName}`);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) return resp.json();
        })
        .then((data) => {
          this.pokemon = data;
        })
        .catch((error) => {
          console.log(req);
          console.log("error");
        });
    },
    fetchEv() {
      let reqEv = new Request(`https://pokeapi.co/api/v2/pokemon-species/${this.pokemonName}`);

      fetch(reqEv)
        .then((resp) => {
          if (resp.status === 200) return resp.json();
        })
        .then((data) => {
          this.pokemonSpecie = data;
        })
        .catch((error) => {
          console.log("error");
        });
    },
  },
  created() {
    this.fetchData();
    this.fetchEv();
  },
};
</script>

<template>
  <div class="box pokemon-page">
    <div class="wrapper page">
      <div class="back-container">
        <router-link to="/"> <i class="fa-solid fa-arrow-left"></i></router-link>
      </div>
      <div class="main-content">
        <div class="sprite-name">
          <h1 class="pokemon-name mobile">{{ pokemonName }}</h1>
          <p class="pokemon-number mobile">
            #
            {{
              pokemon.id >= 100
                ? pokemon.id
                : pokemon.id >= 10
                ? "0" + pokemon.id
                : "00" + pokemon.id
            }}
          </p>
          <div
            class="sprite-container"
            v-bind:style="{
              background: 'var(--background-type-' + pokemon.types[0].type.name + ')',
            }"
          >
            <button @click="shiny = !shiny" :class="[shiny ? 'shiny' : '']">
              See {{ shiny ? "normal sprite" : "shiny sprite" }}
            </button>
            <img
              :src="[
                shiny
                  ? pokemon.sprites.front_shiny
                  : 'https://assets.pokemon.com/assets/cms2/img/pokedex/full/' +
                    [
                      pokemon.id > 100
                        ? pokemon.id
                        : pokemon.id > 10
                        ? '0' + pokemon.id
                        : '00' + pokemon.id,
                    ] +
                    '.png',
              ]"
              alt=""
              class="sprite"
            />
          </div>
        </div>
        <div class="infos">
          <h1 class="pokemon-name desktop">
            {{ pokemonName }} - #
            {{
              pokemon.id > 100 ? pokemon.id : pokemon.id > 10 ? "0" + pokemon.id : "00" + pokemon.id
            }}
          </h1>

          <div class="types">
            <Type
              v-for="(value, index) in pokemon.types"
              :key="'value' + index"
              :typeName="value.type.name"
            />
          </div>
          <h3>Description</h3>
          <p class="text desc">{{ pokemonSpecie.flavor_text_entries[0].flavor_text }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.mobile {
  display: none;
}
.pokemon-page {
  flex-direction: column;
  .wrapper.page {
    position: relative;
    height: 80vh;
    display: flex;
    justify-content: center;
    align-items: center;

    h3 {
      padding: 10px 0px 5px 0px;
      color: var(--text-black);
    }
    .desc {
      text-transform: lowercase;
      &::first-letter {
        text-transform: capitalize;
      }
    }
  }
}

i {
  position: absolute;
  top: 40px;
  left: 0;
  color: #2e3053;
  font-size: 20px;
}

.main-content {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 40px;
  padding-top: 35px;
  .pokemon-name.desktop {
    font-size: 38px;
    // margin-bottom: 20px;
  }
  .pokemon-name {
    margin-bottom: 5px;
    color: var(--text-black);
  }
  .pokemon-number {
    margin-bottom: 20px;
    font-size: 16px;
    font-weight: 200;
  }
  .types {
    display: flex;
    .type {
      text-transform: capitalize;
      &:first-child {
        margin-right: 5px;
      }
    }
  }
}

.dark {
  i {
    color: #f6fbfb !important;
  }
  .pokemon-name,
  h3 {
    color: #f6fbfb !important;
  }
}

.sprite-name {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  width: 100%;
  .sprite-container {
    width: 100%;
    height: 50vh;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 15px;
    position: relative;
    button {
      position: absolute;
      bottom: 20px;
      right: 20px;
      background: white;
      padding: 5px 20px;
      border-radius: 10px;
      border: none;
      cursor: pointer;
    }
    img {
      height: 70%;
      width: auto;
    }
  }
}

.infos {
  padding-top: 10px;
  width: 100%;
  display: flex;
  flex-direction: column;
}

@media screen and(max-width: 840px) {
  .main-content {
    grid-template-columns: 1fr;
    grid-gap: 10px;
  }
  .desktop {
    display: none;
  }
  .mobile {
    display: block;
  }
}
</style>

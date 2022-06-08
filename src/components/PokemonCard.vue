<script setup>
import Type from "./Type.vue";
</script>

<script>
export default {
  props: ["pokemonUrl"],
  data: () => {
    return {
      pokemon: {},
    };
  },
  methods: {
    fetchData() {
      let req = new Request(this.pokemonUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) return resp.json();
        })
        .then((data) => {
          if (data != null) {
            this.pokemon = data;
          } else {
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  created() {
    this.fetchData();
  },
};
</script>

<template>
  <router-link
    v-if="pokemon != undefined"
    :to="{
      name: 'pokemon',
      params: { pokemonName: pokemon.name, pokemonUrl: pokemonUrl },
    }"
    class="pokemon-card wrapper"
    v-bind:style="{
      background: 'var(--background-type-' + this.pokemon.types[0].type.name + ')',
      // boxShadow: '0px 10px 20px 0px var(--background-type-' + pokemon.types[0].type.name + ')',
    }"
  >
    <div class="infos">
      <p class="pokemon-number">
        #{{
          pokemon.id >= 100 ? pokemon.id : pokemon.id >= 10 ? "0" + pokemon.id : "00" + pokemon.id
        }}
      </p>
      <h3 class="pokemon-name">{{ pokemon.name }}</h3>
      <div class="types">
        <Type
          v-for="(value, index) in pokemon.types"
          :key="'value' + index"
          :typeName="value.type.name"
        />
      </div>
    </div>
    <img
      :src="
        'https://assets.pokemon.com/assets/cms2/img/pokedex/full/' +
        [pokemon.id > 100 ? pokemon.id : pokemon.id >= 10 ? '0' + pokemon.id : '00' + pokemon.id] +
        '.png'
      "
      alt=""
      class="sprite"
    />
    <img class="ball" src="../assets/svg/ball.svg" alt="" />
    <img class="dots" src="../assets/svg/dots-2.svg" alt="" />
  </router-link>
</template>

<style lang="scss">
.pokemon-card {
  cursor: pointer;
  position: relative;
  display: flex;
  justify-content: space-between;
  padding: 20px;
  border-radius: 10px;
  overflow: hidden;
  text-decoration: none;
  height: 125px;
  h3 {
    color: var(--text-white);
    margin-bottom: 10px;
    text-transform: capitalize;
  }
  .sprite {
    position: absolute;
    right: -3%;
    height: 151px;
    top: -5%;
    width: auto;
    z-index: 50;
  }
  .ball {
    position: absolute;
    right: -15%;
    top: 0;
  }
  .dots {
    position: absolute;
    top: 0;
    right: 50%;
  }
  .types {
    display: flex;
    .type {
      &:first-child {
        margin-right: 5px;
      }
    }
  }
}
</style>

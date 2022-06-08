<script setup>
import Dex from "../components/Dex.vue";
import HomeHeader from "../components/HomeHeader.vue";
import PokemonCard from "../components/PokemonCard.vue";
import { createToast } from "mosha-vue-toastify";
import "mosha-vue-toastify/dist/style.css";
</script>

<script>
// let list = [];
let list = [];
// list = ["ditto", "palkia"];

export default {
  data: () => {
    return {
      apiUrl: "https://pokeapi.co/api/v2/pokemon/",
      pokemonUrl: "",
      searchvalue: "",
      search: false,
    };
  },
  methods: {
    scrolled() {
      let scrollhead = document.getElementById("top-header");
      scrollhead.classList.add("isHidden");
      document.documentElement.scrollTop || document.body.scrollTop;
      document.documentElement.scrollTop = document.body.scrollTop = 1000;
    },
    setPokemonUrl(url) {
      if (this.searchvalue !== "") {
        this.pokemonUrl = this.apiUrl + this.searchvalue.toLowerCase();

        let req = new Request(
          `https://pokeapi.co/api/v2/pokemon/${this.searchvalue.toLowerCase()}`
        );
        fetch(req)
          .then((resp) => {
            if (resp.status === 200) return resp.json();
          })
          .then((data) => {
            if (data != undefined) {
              list.unshift(this.searchvalue.toLocaleLowerCase());
              this.searchvalue = "";
              if (this.search == false) {
                let scrollhead = document.getElementById("top-header");
                scrollhead.classList.remove("isHidden");
                this.search = true;
              } else {
              }
            } else {
              createToast("Pokémon not found, check spelling and try again", {
                position: "bottom-right",
                type: "danger",
              });
              this.searchvalue = "";
            }
          });
      } else {
        createToast("Please enter the name of a Pokémon", {
          position: "bottom-right",
          type: "danger",
        });
      }
    },
  },
};
</script>

<template>
  <main>
    <div class="top" id="top-header">
      <div class="bg">
        <!-- <svg
          width="414"
          height="207"
          viewBox="0 0 414 207"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M207 -207C313.525 -207 401.396 -127.727 414 -25.3742H312.97C301.466 -72.8752 258.386 -108.174 207 -108.174C155.614 -108.174 112.534 -72.8752 101.03 -25.3742H0C12.604 -127.727 100.475 -207 207 -207Z"
            fill="url(#paint0_linear_23608_3248)"
          />
          <path
            d="M312.97 25.3742H414C401.396 127.727 313.525 207 207 207C100.475 207 12.604 127.727 0 25.3742H101.03C112.534 72.8751 155.614 108.174 207 108.174C258.386 108.174 301.466 72.8751 312.97 25.3742Z"
            fill="url(#paint1_linear_23608_3248)"
          />
          <path
            d="M207 68.1096C244.898 68.1096 275.62 37.6159 275.62 0C275.62 -37.6159 244.898 -68.1097 207 -68.1097C169.102 -68.1097 138.38 -37.6159 138.38 0C138.38 37.6159 169.102 68.1096 207 68.1096Z"
            fill="url(#paint2_linear_23608_3248)"
          />
          <defs>
            <linearGradient
              id="paint0_linear_23608_3248"
              x1="207"
              y1="0"
              x2="207"
              y2="185.5"
              gradientUnits="userSpaceOnUse"
            >
              <stop stop-color="#F5F5F5" />
              <stop offset="1" stop-color="white" />
            </linearGradient>
            <linearGradient
              id="paint1_linear_23608_3248"
              x1="207"
              y1="0"
              x2="207"
              y2="185.5"
              gradientUnits="userSpaceOnUse"
            >
              <stop stop-color="#F5F5F5" />
              <stop offset="1" stop-color="white" />
            </linearGradient>
            <linearGradient
              id="paint2_linear_23608_3248"
              x1="207"
              y1="0"
              x2="207"
              y2="185.5"
              gradientUnits="userSpaceOnUse"
            >
              <stop stop-color="#F5F5F5" />
              <stop offset="1" stop-color="white" />
            </linearGradient>
          </defs>
        </svg> -->
      </div>
      <HomeHeader />
      <div class="search-container box">
        <div class="wrapper">
          <div class="form">
            <form @submit.prevent="setPokemonUrl" :apiUrl="apiUrl">
              <input
                type="text"
                placeholder="What Pokémon are you looking for?"
                v-model="searchvalue"
              />
              <button class="ico">
                <i class="fa-solid fa-magnifying-glass"></i>
              </button>
            </form>
            <button class="ico clear" @click="search = false" :class="search ? '' : 'disabled'">
              <i class="fa-solid fa-trash"></i>
            </button>
          </div>
        </div>
      </div>
      <div class="box">
        <div div class="wrapper" v-if="list == []">
          <h6>Search for a Pokémon to create a list</h6>
        </div>
      </div>
    </div>
    <Dex v-if="search == false" :apiUrl="apiUrl" />
    <div class="box history" @if="search == true" @scroll="scrolled()">
      <div class="wrapper">
        <div class="box historic-container">
          <PokemonCard
            v-for="pokemon in list"
            :key="pokemon"
            :pokemonUrl="apiUrl + pokemon"
            @click="search = false"
          />
        </div>
      </div>
    </div>
  </main>
</template>

<style lang="scss">
.top {
  position: absolute;
  z-index: 200;
  background: #f6fbfba3;
  width: 100%;
  backdrop-filter: blur(62px);
  padding-bottom: 20px;
  transition: all 0.3s;
  &.isHidden {
    transform: translateY(-130px);
  }
}

.dark {
  .top {
    background: #1a202cb6;
  }
}

main {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.history {
  padding-top: 25vh;
  flex-grow: 1;
  overflow-y: scroll;
  .wrapper {
    height: 100%;
  }
}

.historic-container,
.list {
  margin-top: 20px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  padding-top: 15px;
  overflow: scroll;

  grid-gap: 25px;
  .wrapper {
    width: 100%;
  }
}

h6 {
  margin-top: 20px;
  text-align: center;
  color: var(--text-grey);
  font-weight: 100;
  font-size: 15px;
}

.form {
  display: flex;
  gap: 10px;
  button {
    cursor: pointer;
  }
  .clear {
    background: #e53e3e !important;
    &.disabled {
      opacity: 0.7;
      &:hover {
        background: #e53e3e !important;
        cursor: not-allowed;
      }
    }
    &:hover {
      background: #9b2c2c !important;
    }
  }
}

.search-container {
  position: relative;
  z-index: 10;
  width: 100%;
  .wrapper {
    form {
      display: flex;
      gap: 10px;
      width: 100%;
    }
    input {
      flex-grow: 1;
    }
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
        background-color: #37394c;
      }
    }
  }
}
.dark {
  input[type="text"] {
    background: rgb(31, 41, 54);
    color: white;
    &:focus {
      background: rgb(40, 53, 70);
      outline: none;
    }
  }
}

@media screen and (max-width: 580px) {
  .history {
    padding-top: 28vh;
  }
}

@media screen and(max-width: 1050px) {
  .historic-container,
  .list {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media screen and(max-width: 700px) {
  .historic-container,
  .list {
    grid-template-columns: 1fr;
  }
}

@media screen and(min-width: 1600px) {
  .historic-container,
  .list {
    grid-template-columns: repeat(4, 1fr);
  }
}
</style>

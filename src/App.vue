<script setup>
import { RouterLink, RouterView } from "vue-router";
</script>

<script>
export default {
  mounted() {
    let app = document.getElementById("mc");

    const setCurrentTheme = () => {
      if (window.matchMedia && window.matchMedia("(prefers-color-scheme: dark)").matches) {
        app.classList.add("dark");
        app.classList.remove("light");
      } else if (window.matchMedia && window.matchMedia("(prefers-color-scheme: light)").matches) {
        app.classList.add("light");
        app.classList.remove("dark");
      }
    };

    setCurrentTheme();

    window.matchMedia("(prefers-color-scheme: dark)").addEventListener("change", (event) => {
      if (sessionStorage.getItem("theme") == "dark" || sessionStorage.getItem("theme") != "light") {
        sessionStorage.setItem("theme", event.matches ? "dark" : "light");
        app.classList.add("dark");
        app.classList.remove("light");
      } else {
        app.classList.add("light");
        app.classList.remove("dark");
      }
    });
  },
};
</script>

<template>
  <div class="main-container" id="mc">
    <RouterView />
  </div>
</template>

<style lang="scss">
@import url(https://fonts.googleapis.com/css?family=DM+Sans:regular,italic,500,500italic,700,700italic);

// Variables

:root {
  // Types
  --type-bug: #8cb230;
  --type-dark: #58575f;
  --type-dragon: #0f6ac0;
  --type-electric: #eed535;
  --type-fairy: #ed6ec7;
  --type-fighting: #d04164;
  --type-fire: #fd7d24;
  --type-flying: #748fc9;
  --type-ghost: #556aae;
  --type-grass: #62b957;
  --type-ground: #dd7748;
  --type-ice: #61cec0;
  --type-normal: #9da0aa;
  --type-poison: #a552cc;
  --type-psychic: #ea5d60;
  --type-rock: #baab82;
  --type-steel: #417d9a;
  --type-water: #4a90da;

  // Background Types
  --background-type-bug: #8bd674;
  --background-type-dark: #6f6e78;
  --background-type-dragon: #7383b9;
  --background-type-electric: #f2cb55;
  --background-type-fairy: #eba8c3;
  --background-type-fighting: #eb4971;
  --background-type-fire: #ffa756;
  --background-type-flying: #83a2e3;
  --background-type-ghost: #8571be;
  --background-type-grass: #8bbe8a;
  --background-type-ground: #f78551;
  --background-type-ice: #91d8df;
  --background-type-normal: #b5b9c4;
  --background-type-poison: #9f6e97;
  --background-type-psychic: #ff6568;
  --background-type-rock: #d4c294;
  --background-type-steel: #4c91b2;
  --background-type-water: #58abf6;

  // Background
  --background-white: #ffffff;
  --background-default-input: #ecf3f5;
  --background-pressed-input: #e7eff1;
  --background-modal: #17171b80;
  --background-dark: #1a202c;

  // Text
  --text-white: #ffffff;
  --text-black: #2e3053;
  --text-grey: #7f7f9e;
  --text-number: #17171b99;

  // Height
  --height-short: #ffc5e6;
  --height-medium: #aebfd7;
  --height-tall: #aaacb8;

  // Weight
  --weight-lite: #99cd7c;
  --weight-normal: #57b2dc;
  --weight-heavy: #5a92a5;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "DM Sans";
}

// Utils

#app {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  // background: linear-gradient(119.34deg, #eff1f8 0%, #d9ddeb 100%);
}

.main-container {
  background: #f6fbfb;
  width: 100%;
  // max-width: 414px;
  height: 100vh;
  // max-height: 890px;
  // box-shadow: 0px 10px 20px rgba(0, 0, 0, 0.108);
  // border-radius: 30px;
  overflow: hidden;
  &.dark {
    background: var(--background-dark);
  }
}

::placeholder {
  color: var(--text-grey);
}

input[type="text"] {
  transition: all 0.3s;
  background: var(--background-default-input);
  border: none;
  border-radius: 10px;
  padding: 20px 25px;
  color: var(--text-black);
  &:focus {
    background: var(--background-pressed-input);
    outline: none;
  }
}

.box {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  .wrapper {
    width: 85%;
  }
}

.title {
  font-size: 32px;
}

.subtitle {
  font-size: 16px;
  font-weight: 500;
}

.pokemon-name {
  font-size: 26px;
  font-weight: bold;
  color: var(--text-white);
  text-transform: capitalize;
}

.pokemon-number {
  font-size: 12px;
  font-weight: bold;
  color: var(--text-number);
}

.pokemon-type {
  font-size: 15px;
}

.text {
  color: var(--text-grey);
}
</style>

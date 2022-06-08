<script>
export default {
  props: ["typeName"],
};
</script>

<template>
  <div class="type" :style="{ background: `var(--type-${typeName})` }">
    <span :class="typeName">
      <img v-bind:src="'/src/assets/icons-types/' + typeName + '.svg'" />
      <p class="pokemon-type">{{ typeName }}</p>
    </span>
  </div>
</template>

<style lang="scss">
.type {
  padding: 5px;
  border-radius: 3px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--type-grass);
  color: var(--text-white);
  text-transform: capitalize;
  span {
    display: flex;
    align-items: center;
    justify-content: center;
    img {
      margin-right: 5px;
      height: 15px;
    }
  }
}
</style>

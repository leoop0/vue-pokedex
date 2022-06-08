<script></script>

<template>
  <div class="title-container box">
    <div class="wrapper">
      <h1 class="title">Pokédex</h1>
      <p class="text">Search for Pokémon by name or using the National Pokédex number.</p>
    </div>
  </div>
</template>

<style lang="scss">
.title-container {
  position: relative;
  z-index: 10;
  padding-top: 50px;
  transform: translateY(0);
  transition: transform 300ms linear;
  &.hidden {
    transform: translateY(-100%);
  }
  h1 {
    margin-bottom: 10px;
    color: var(--text-black);
  }
  p {
    margin-bottom: 20px;
  }
}

.dark {
  h1 {
    color: #f6fbfb;
  }
}
</style>

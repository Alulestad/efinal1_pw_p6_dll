<template>
  <div class="container">
    <img :src="imagen" alt="pokemones" />
    <label>{{ nombre }}</label>
  </div>
</template>

<script>
export default {
  props: {
    imagen: {},
    nombre: {
      type: String,
      default: "XXXXXX",
    },
  },
  data() {
    return {};
  },
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 200px;
  margin: 0 auto;
  background-color: rgb(255, 255, 255);
}

img {
  width: 200px;
  height: 200px;
  background-color: rgb(255, 255, 255);
}
</style>

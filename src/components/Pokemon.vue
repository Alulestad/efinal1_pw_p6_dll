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
  width: 300px;
  margin: 0 auto;
}

img {
  width: 250px;
  height: 250px;
}
</style>

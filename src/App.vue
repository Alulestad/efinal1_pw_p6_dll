<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
  <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
  <div class="containerGeneral" v-if="juegoPrincipal">
    <div class="puntajes">
      <label>Puntaje: {{ puntaje }}</label>
      <label>Intento: {{ intento }}</label>
    </div>
    <div class="imagenesPok">
      <Pokemon
        v-for="(imagen, indice) in imagenes"
        :key="indice"
        :imagen="imagen"
        :nombre="obtenidosGet[indice]"
      />
    </div>
    <div class="boton_jugar">
      <button @click="obtenerPokes">Jugar</button>
    </div>
  </div>
  <div class="containerMensaje" v-if="!juegoPrincipal">
    <div v-if="!juegoPrincipal">
      <div class="pantalla_gano" v-if="gano">
        <label
          >Puntaje: {{ puntaje }} Felicitaciones has ganado un premio de
          $10.000,00</label
        >
      </div>
      <div class="pantalla_perdio" v-else>
        <label
          >Has utilizado tus 5 intentos El juego ha termindo, intentalo
          nuevamente</label
        >
      </div>
      <div class="btn_reset">
        <button @click="resetear">Nuevo Juego</button>
      </div>
    </div>
  </div>
</template>

<script>
import Pokemon from "./components/Pokemon.vue";

export default {
  name: "App",
  components: {
    Pokemon,
  },
  data() {
    return {
      puntaje: 0,
      intento: 0,
      obtenidosGet: [],
      imagenes: [
        "https://via.placeholder.com/250",
        "https://via.placeholder.com/250",
        "https://via.placeholder.com/250",
      ],

      juegoPrincipal: true,
      gano: false,
    };
  },

  methods: {
    async obtenerPokes() {
      const tmpImagenes = [];
      const tmpNames = [];

      for (let i = 0; i < this.imagenes.length; i++) {
        let tmp = Math.floor(Math.random() * (9 - 5 + 1)) + 5;
        let tmpImagen =
          "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/" +
          tmp +
          ".svg";
        console.log("imagen: " + tmpImagen);
        let tmpIname = "https://pokeapi.co/api/v2/pokemon/" + tmp;
        console.log("name: " + tmpIname);
        const { forms } = await fetch(tmpIname).then((res) => res.json());
        console.log("forms: " + forms);
        const { name } = forms.pop();
        console.log("name: " + name);
        tmpImagenes.push(tmpImagen);
        tmpNames.push(name);
      }

      this.intento++;
      let tmpCoincidencias = 0;
      this.imagenes = tmpImagenes;
      this.obtenidosGet = tmpNames;

      if (
        this.obtenidosGet[0] === this.obtenidosGet[1] &&
        this.obtenidosGet[0] === this.obtenidosGet[2]
      ) {
        tmpCoincidencias = 5;
      } else if (
        this.obtenidosGet[0] === this.obtenidosGet[1] ||
        this.obtenidosGet[1] === this.obtenidosGet[2] ||
        this.obtenidosGet[0] === this.obtenidosGet[2]
      ) {
        tmpCoincidencias = 3;
      } else {
        tmpCoincidencias = 0;
      }

      console.log("Coincidencias: " + tmpCoincidencias);
      this.puntaje = this.puntaje + tmpCoincidencias;

      if (this.intento >= 5 || this.puntaje >= 10) {
        this.juegoPrincipal = false;
        if (this.puntaje >= 10) {
          this.gano = true;
        } else {
          this.gano = false;
        }
      }
    },
    resetear() {
      this.puntaje = 0;
      this.intento = 0;
      this.obtenidosGet = [];
      this.imagenes = [
        "https://via.placeholder.com/250",
        "https://via.placeholder.com/250",
        "https://via.placeholder.com/250",
      ];
      this.juegoPrincipal = true;
      this.gano = false;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.imagenesPok {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.containerGeneral {
  border: 3px solid black;
  margin: 50px 150px;
}

.imagenesPok {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: row;
}
.containerMensaje {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  border: 3px solid black;
  margin: 10px auto;
  width: 450px;
}
label {
  margin: 5px;
}

.pantalla_gano {
  color: blue;
}

.pantalla_perdio {
  color: red;
}

button {
  border: 3px solid black;
}
</style>

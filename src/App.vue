<template>
  <div id="app">
    <div id="datosPoke" class="container">
      <img src="./assets/logopokemon.png" alt="logo-pokemon" class="logo" />

      <div id="ingresoPoke">
        <input
          class="input"
          type="text"
          v-model="pokemones.name"
          placeholder="Ingresa el Pokémon a buscar o su número"
        />
        <button @click="buscarPokemon" class="btnpoke">PokeBusqueda</button>
      </div>
      <!--nombre y foto-->
      <div id="nombrefoto_pokemon">
        <p id="titulo_pokemon">{{ getNombre }}</p>
        <img :src="getFoto" alt="foto-poke" class="fotoPoke" />
      </div>

      <!--movimientos-->
      <div id="mov-hab">
        <div id="movimientos">
          <h2 class="mov_poke">Movimientos que puede aprender</h2>
          <ul>
            <li
              class="texto1"
              v-for="movimiento in getMovimientos"
              :key="movimiento"
            >
              {{ movimiento.move.name }}
            </li>
          </ul>
        </div>

        <!--habilidades-->
        <div id="habilidades">
          <h2 class="hab_pokemon">Habilidades del Pokemon</h2>
          <ul>
            <li
              class="texto1"
              v-for="habilidad in getHabilidades"
              :key="habilidad"
            >
              {{ habilidad.ability.name }}
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      pokemones: {
        name: "pikachu",
        moves: [],
        abilities: [],
        sprites:{
          front_default:''
        }
      },
    };
  },
 
  methods: {
    
    buscarPokemon() {
      let nombreP = this.pokemones.name.toLowerCase();
      fetch(`https://pokeapi.co/api/v2/pokemon/${nombreP}`)
        .then((resp) => resp.json())
        .then((data) => {
          this.pokemones = data;
        });
    },
    
  },
   created() {
    let name = this.pokemones.name
      fetch(`https://pokeapi.co/api/v2/pokemon/${name}`)
        .then((resp) => resp.json())
        .then((data) => {
          this.pokemones = data;
        });
        console.log('hola')
    },
  computed: {
    getNombre() {
      return this.pokemones.name;
    },

    getMovimientos() {
      return this.pokemones.moves.splice(0, 10);
    },
    getHabilidades() {
      return this.pokemones.abilities.splice(0, 10);
    },
    getFoto() {
      if (this.pokemones.sprites) {
        return this.pokemones.sprites.front_default;
      } else {
        return "";
      }
    },
  },
};
</script>
<style lang="scss">
$color-1: #ffcb08;
$color-2: #3268b1;
$color-boton: #f21313;
$destacar: #81a1e9;
$info: "Baloo Thambi 2", cursive;
$subtitulo: "Fredoka One", cursive;
$titulo: "Luckiest Guy", cursive;
$boton: "Righteous", cursive;
body{
  background-image: url('https://i.imgur.com/xO0wn1D.png');
  background-position: center;
  background-repeat: no-repeat;
  
}
.logo {
  width: 60%;
  height: 60%;
}

#ingresoPoke {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 30px;
  .btnpoke {
    background-color: $color-boton;
    height: 30px;
    border-radius: 0px 20px 20px 0px;
    font-family: $boton;
  }
  .btnpoke:hover {
    background-color: $destacar;
  }
  .input {
    width: 500px;
    height: 24px;
    font-family: $titulo;
  }
  .input:focus {
    outline-width: 2px;
    outline-style: outset;
    outline-color: $destacar;
  }
}
#datosPoke {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  box-sizing: border-box;
}

#titulo_pokemon {
  font-family: $titulo;
  color: $color-1;
  -webkit-text-stroke: 3px $color-2;
  font-size: 50px;
  text-align: center;
  margin: 0;
}
#nombrefoto_pokemon {
  display: flex;
  flex-direction: column;

  align-items: center;
  .fotoPoke {
    width: 200px;
    height: 200px;
  }
}
#mov-hab {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  .hab_pokemon,
  .mov_poke {
    font-family: $subtitulo;
    font-size: 1rem;
    margin: 0 20px;
  }
  .texto1 {
    font-family: $info;
    font-size: 1rem;
  }
}
</style>

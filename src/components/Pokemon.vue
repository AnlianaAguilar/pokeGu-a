<template>
  <div>    
    <img src="https://upload.wikimedia.org/wikipedia/commons/9/98/International_Pok%C3%A9mon_logo.svg" alt="">
    <h1>PokéGuia</h1>
    <form>
      <label for="">Nombre</label>
      <input type="text" v-model="characters.name">
      <input type="submit" @click.prevent="fetchPokeMon" placeholder="Ingrese nombre de Pokemon">
    </form>
    <section>
      <img :src="img" alt="">
      <!-- <h1>Nombre:{{characters.name}}</h1> -->
      <h1>Movimientos:</h1>
      <p v-for="(move) in theMoves" :key="move">{{move}}</p>
      <h1>Habilidades</h1>
      <p v-for="(ability, i) in theAbilities" :key="i">{{ability}}</p>
    </section>
  </div>
</template>

<script>
export default {
  name: "pokemon-comp",
  // props: {},
  data: function () {
    return {
      characters:{
        name:'ditto',
        moves:[],
        abilities:[],
        imagen:""
      }
    };
  },
  computed: {
    img(){
      return this.characters.imagen
    },
    theAbilities(){
      return this.characters.abilities.map(function(elemt){
          return elemt.ability.name
        })
    },
    theMoves(){
      return this.characters.moves.map(function(elemt){
          return elemt.move.name
        })
    }
  },
  methods: {
    async fetchPokeMon(){ // puede que demore
      try{
        const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${this.characters.name}`) //se detiene el codigo(macro task)
        if(!response.ok) throw ('Error en la solicitud')

        const json = await response.json() // cambia el texto plano(json valido) a obj de javascript
        console.log(json)
        
        this.characters.name = json.name
        console.log(this.characters.name) 

        this.characters.moves = json.moves
        console.log("moves",this.characters.moves)

        this.characters.abilities = json.abilities
        console.log("habilidad",this.characters.abilities)

        this.characters.imagen = json.sprites.front_default
        console.log(this.characters.imagen)
      
      }
      catch(error){
        console.log(error)
      }
    },

      // let nameAbilities = this.abilities.map(function(ability){
      //   return ability.name
      // })
      //console.log(nameAbilities)

  },
  // watch: {},
  // components: {},
  // mixins: [],
  // filters: {},
  // -- Lifecycle Methods
  created(){
    this.fetchPokeMon()
  }
  // -- End Lifecycle Methods
};
</script>

<style scoped>
</style>

<!--








<div> con overflow-y: scroll, y alto fijo por ejemplo.
-->
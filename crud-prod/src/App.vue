<template>
  <div id="app">
   
    <img width="30" src="./assets/logo.png">
  
  <!--   <HelloWorld msg="Welcome to Your Vue.js App"/> 
  -->
    <h1>CRUD - Products</h1>

    <form v-on:submit.prevent="addGame">

      <div class="row">
        <div class="col-md-6">
          <div class="form-group">
            <label>nombre:</label>
            <input type="text" class="form-control" v-model="games.nombre"/>
          </div>
        </div>
        </div>

      <div class="row">
        <div class="col-md-6">
          <div class="form-group">
            <label>Plataforma:</label>
            <input type="text" class="form-control" v-model="games.plataforma"/>
          </div>
        </div>
        </div>

      <div class="row">
        <div class="col-md-6">
          <div class="form-group">
            <label>Precio:</label>
            <input type="text" class="form-control" v-model="games.precio"/>
          </div>
        </div>
        </div>
       
       <br />
        <div class="form-group">
          <button class="btn btn-primary">Create Game</button>
        </div>
      <br/>
           
    </form>
      <div class="form-group">

          <button @click="updateGame()">
              Update Game
          </button>
        </div>

  <ul v-if="posts && posts.length">
    <li v-for="game of posts">
       {{ game.id }} - 
       {{ game.nombre }}
              <button @click="getonegame(game.id)">
                edit
     </button>
     <button @click="deleteGame(game.id)">
                delete
     </button>
    </li>
  </ul>

  <ul v-if="errors && errors.length">
    <li v-for="error of errors">
      {{error.message}}
    </li>
  </ul>
  </div>

</template>

<script>
// import HelloWorld from './components/HelloWorld.vue' 
import axios from 'axios';

export default {
  name: 'app',
  data() {
    return {
      games: {},
      posts: [],
      errors: []
    }
  },
  created() {
    // Fetches posts when the component is created.
    this.getgames();
 
  } ,
  methods: {
 
  getgames() {
    axios.get('http://35.225.62.24:8082/api/juegos')
    .then(response => {
      // JSON responses are automatically parsed.
      this.posts = response.data
    })
    .catch(e => {
      this.errors.push(e)
    })
  },

  getonegame(idgame) {
    axios.get('http://35.225.62.24:8082/api/juegos/'+ idgame)
    .then(response => {
      // JSON responses are automatically parsed.
      this.games = response.data
      alert("status: " + response.status + ", selected: " + JSON.stringify(response.data));

    })
    .catch(e => {
      this.errors.push(e)
    })
  },

  addGame() {
    alert(this.games.nombre + this.games.plataforma + this.games.precio);
    axios.post('http://35.225.62.24:8082/api/juegos',  this.games)
    .then(response => {
      alert("status: " + response.status + ", inserted: " + JSON.stringify(response.data));
    })
    .catch(e => {
      this.errors.push(e)
    })
  },
  
  updateGame() {
    //alert(idgame);
    alert(this.games.id);
    axios.put('http://35.225.62.24:8082/api/juegos/' + this.games.id,  this.games)
    .then(response => {
      alert("status: " + response.status + ", updated: " + JSON.stringify(response.data));
      this.getgames();

    })
    .catch(e => {
      this.errors.push(e)
    })
  },
  

  deleteGame(idgame) {
    alert("delete" + idgame);
    
    axios.delete('http://35.225.62.24:8082/api/juegos/' + idgame)
    .then(response => {

      alert("status: " + response.status + ", deleted: " + JSON.stringify(response.data));
      this.getgames();

    })
    .catch(e => {
      this.errors.push(e)
    })
  },
  
  }

}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

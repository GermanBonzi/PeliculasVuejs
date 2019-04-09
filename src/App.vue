<template>
  <div id="app">
      <b-navbar class="navbar" toggleable="md" type="dark" >
        <b-button v-on:click="Peliculas" variant="warning"> Inicio </b-button>
        <b-button class="boton" v-on:click="Populares" variant="warning">Populares</b-button>
        <b-navbar-nav class="ml-auto">
          <b-nav-form>
            <b-form-input size="sm" class="mr-sm-2" type="text" placeholder="Buscar" id="buscado"></b-form-input>
            <b-button size="sm" variant="warning" class="boton buscar" v-on:click="Buscar">Buscar</b-button>
          </b-nav-form>
     </b-navbar-nav>
   </b-navbar>
	    <div>
        <li v-for="item in peliculas" class="lista list-group-item">
          <div>
            <h5>{{item.original_title}}</h5>
            <b-card style="max-width: 20rem;" class="card">
              <img :src="imageUrl + item.poster_path" style="width:17rem; height:25rem;">
                <b-card-text>
                  <p class="fecha"> {{item.release_date}}</p>
                </b-card-text>
            </b-card>
          </div>
          <div>
            <b-button variant="warning" v-on:click="Trailer(item.id)"> Trailer </b-button>
          </div>
       </li>
      </div>
  </div>
</template>
<script>
	import Vue from 'vue'
  import axios from 'axios'
	import} BootstrapVue from 'bootstrap-vue'
	import 'bootstrap/dist/css/bootstrap.css'
	import 'bootstrap-vue/dist/bootstrap-vue.css'
	Vue.use(BootstrapVue);
	export default {
		name: "app",
		 created: function() {
			this.Peliculas();
		},
		data () {
			return {
				imageUrl: 'https://image.tmdb.org/t/p/w342',
				 peliculas: [],
         trailer:[]
			}
		},
		methods: {
        Peliculas(){
                 axios.get('https://api.themoviedb.org/3/discover/movie?api_key=1b62ccff88d2cd537027e1d82920197b&primary_release_date.gte=2018-05-15&primary_release_date.lte=2018-7-28').then((response)=>{
                     this.peliculas = response.data.results;
                     console.log(this.peliculas)
                });
          },
        Buscar (){
          var buscado = document.getElementById("buscado").value;
                axios.get('https://api.themoviedb.org/3/search/movie?include_adult=false&page=1&api_key=1b62ccff88d2cd537027e1d82920197b&query='+buscado+'&language=en-US'+
                    '').then((response)=>{
                     this.peliculas = response.data.results;
                });
          },

          Populares(){
            axios.get('https://api.themoviedb.org/3/discover/movie?api_key=b835ed932f4f0a4532e2512ff3cce439&sort_by=popularity.desc')
            .then(response => (this.peliculas = response.data.results))
          },


          Trailer(trailer){
           axios.get('http://api.themoviedb.org/3/movie/'+trailer+'/videos?api_key=1b62ccff88d2cd537027e1d82920197b').then((response)=>{
                this.trailer = response.data.results[0].key;
                console.log(this.trailer);
                this.url = `https://www.youtube.com/watch?v=${this.trailer}`,
                console.log(this.url),
                window.open(this.url)
           });
         }

		}
	}
</script>

<style>


.lista {
  top: -20px;
  margin: 40px;
  display:  inline-table;
  vertical-align: top;
  width: 120px;
  position: relative;
  font-family: Arial, sans-serif
}
.mb-2{
  background-image:repeating-radial-gradient('./assets/starwars.png');
}

.fecha{
  font-style: italic;

}
h5, .h5{
  font-family: fantasy;
}
.boton{
  position: sticky;
}



.navbar{
background-color:inherit;
}
#app{
background-image: url('./assets/starwars.jpg');
background-position: center;
background-blend-mode:overlay;
}
</style>

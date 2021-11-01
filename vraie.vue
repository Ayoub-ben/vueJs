<template>
  <div>
    <h2>{{msg}}</h2>
    <!--<button v-on:click="getRestaurantsFromServer()"> Test récupération des restaurants</button>-->
    <form @submit.prevent="ajouterRestaurant(event)">
      <label>
        Nom : <input name="name" type="text" required v-model="nom">
      </label>
      <label>
        Cuisine : <input name="cuisine" type="text" required v-model="cuisine">
      </label>
      <label>
        Ville : <input name="ville" type="text" required v-model="ville">
      </label>

      <button>Ajouter</button>
    </form>

    <h1>Nombre de restaurants : {{nbRestaurantTotal}}</h1>
    <p>Chercher par nom <input  @input="chercherRestaurants()" type="text" v-model="nomRestaurantRecherhe" ></p>
    <p>Nombre de pages : {{nbPagesTotal}}</p>
    <p>Nb restaurants par page
      <input @input="chercherRestaurants()" type="range" min=2 max=100 v-model="pagesize">{{pagesize}}
    </p>

    <md-button class="md-raised" :disabled="page===0" @click="pagePrecedente()">Précedent</md-button>
    <md-button class="md-raised" :disabled="page===nbPagesTotal" @click="pageSuivante()">Suivante</md-button>
    &nbsp; Page courante : {{page}}

    <md-table v-model="restaurants"  md-sort="name"  md-sort-order="asc">
      <md-table-row>
        <md-table-head>Nom</md-table-head>
        <md-table-head>Cuisine </md-table-head>
        <md-table-head>Ville </md-table-head>
      </md-table-row>
      <md-table-row  
      
      slot="md-table-row" slot-scope="{item, index}"
      :style="{backgroundColor : getColor(index)}"
      :class="{bordureRouge : (index===2)}">
        <md-table-cell md-label="Name" md-sort-by="name">{{item.name}}</md-table-cell>
        <md-table-cell md-label="Cuisine" md-sort-by="name"> {{item.cuisine}}</md-table-cell>
        <md-table-cell md-label="Ville" md-sort-by="name"> {{item.borough}}</md-table-cell>
        <md-table-cell md-label="Action" md-sort-by="name"> <router-link :to="'/restaurant/' + item._id"> Details</router-link> </md-table-cell>
      </md-table-row>
    </md-table>

<md-dialog-alert
      :md-active="AucunRestaurant"
      @click="RechercheA0()"
      md-title="Post created!"
      md-content="ca marche pas chacal" />

 <md-menu md-size="small" :style="{backgroundColor : getColor(index)}">
      <md-button md-menu-trigger>Small</md-button>

      <md-menu-content>
        <md-menu-item>My Item 1</md-menu-item>
        <md-menu-item>My Item 2</md-menu-item>
        <md-menu-item>My Item 3</md-menu-item>
      </md-menu-content>
    </md-menu>

    <md-menu md-size="medium">
      <md-button md-menu-trigger>Medium</md-button>

      <md-menu-content>
        <md-menu-item>My Item 1</md-menu-item>
        <md-menu-item>My Item 2</md-menu-item>
        <md-menu-item>My Item 3</md-menu-item>
      </md-menu-content>
    </md-menu>

    <md-menu md-size="big">
      <md-button md-menu-trigger>Big</md-button>

      <md-menu-content>
        <md-menu-item>My Item 1</md-menu-item>
        <md-menu-item>My Item 2</md-menu-item>
        <md-menu-item>My Item 3</md-menu-item>
      </md-menu-content>
    </md-menu>

    <md-menu md-size="huge">
      <md-button md-menu-trigger>Huge</md-button>

      <md-menu-content>
        <md-menu-item>My Item 1</md-menu-item>
        <md-menu-item>My Item 2</md-menu-item>
        <md-menu-item>My Item 3</md-menu-item>
      </md-menu-content>
    </md-menu>

    <md-menu md-size="auto">
      <md-button md-menu-trigger>Auto</md-button>

      <md-menu-content>
        <md-menu-item>My Item 1</md-menu-item>
        <md-menu-item>This content is long enough</md-menu-item>
        <md-menu-item>My Item 3</md-menu-item>
      </md-menu-content>
    </md-menu>


  </div>
</template>

<script>
  import _ from "lodash";

  export default {
    name: 'ListeDesRestaurants',

    data: function () {
      return {
        restaurants: [],
        nom: '',
        cuisine: '',
        nbRestaurantTotal: 0,
        page: 0,
        pagesize: 10,
        nbPagesTotal: 0,
        msg: "",
        nomRestaurantRecherhe: "",
        ville: '',
        AucunRestaurant: false,
      }
    },
    mounted() {
      console.log("AVANT AFFICHAGE");
      this.getRestaurantsFromServer();
    },
    methods: {
      pagePrecedente() {
        if (this.page === 0) return;
        this.page--;
        this.getRestaurantsFromServer();
        
      },
      pageSuivante() {
        if (this.page === this.dernierePage) return;
        this.page++;
        this.getRestaurantsFromServer();
      },
      getRestaurantsFromServer() {
        let url = "http://localhost:8080/api/restaurants?";
        url += "page=" + this.page;
        url += "&pagesize=" + this.pagesize;
        url += "&name=" + this.nomRestaurantRecherhe;
        fetch(url)
          .then((responseJSON) => {

            responseJSON.json().then((resJS) => {
              // Maintenant res est un vrai objet JavaScript
              this.restaurants = resJS.data;
              this.nbRestaurantTotal = resJS.count;
              this.AucunRestaurant = false;
            if(this.nbRestaurantsTotal === 0){
              this.AucunRestaurant = true;
            }
              this.nbPagesTotal = Math.round(this.nbRestaurantTotal / this.pagesize);
            });
          })
          .catch(function (err) {
            console.log(err);
          });
      },
      
      supprimerRestaurant(r) {
        let url = "http://localhost:8080/api/restaurants/" + r._id;

        fetch(url, {
          method: "DELETE",
        })
          .then((responseJSON) => {
            console.log(responseJSON)
            responseJSON.json()
              .then((resJS) => {
                // Maintenant res est un vrai objet JavaScript
                console.log(resJS.msg);
                this.msg = resJS.msg;
                this.getRestaurantsFromServer();
              });
          })
          .catch(function (err) {
            console.log(err);
          });
      },
      chercherRestaurants:
        _.debounce(function () {
          this.getRestaurantsFromServer();
        }, 300),
      ajouterRestaurant(event) {
        let form = event.target;

        // Récupération des valeurs des champs du formulaire
        // en prévision d'un envoi multipart en ajax/fetch
        let donneesFormulaire = new FormData(form);

        let url = "http://localhost:8080/api/restaurants";

        fetch(url, {
          method: "POST",
          body: donneesFormulaire
        })
          .then((responseJSON) => {
            responseJSON.json()
              .then((resJS) => {
                // Maintenant res est un vrai objet JavaScript
                console.log(resJS.msg);
                this.msg = resJS.msg;
                this.getRestaurantsFromServer();
              });
          })
          .catch(function (err) {
            console.log(err);
          });
        this.nom = "";
        this.cuisine = "";
        this.ville = "";
      },
      getColor(index) {
        return (index % 2) ? 'lightBlue' : 'pink';
      },
       RechercheA0(){
      this.AucunRestaurant = false;
    }
    },
    

  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1 {
    background-color: yellow;
  }
</style>
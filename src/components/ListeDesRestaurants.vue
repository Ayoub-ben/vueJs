<template>
  <div>
    <h2>{{ msg }}</h2>
    <form class="ajouter" v-on:submit.prevent="ajouterRestaurant(event)">
      <label>
        Nom : <input name="name" type="text" required v-model="nom" />
      </label>
      <label>
        Cuisine :
        <input name="cuisine" type="text" required v-model="cuisine" />
      </label>

      <button>Ajouter</button>
    </form>

    <h1>Nombre de restaurants : {{ nbRestaurantsTotal }}</h1>
    <p>
      Chercher par nom :
      <input
        @input="chercherRestaurants"
        type="texte"
        v-model="nomRestauRecherche"
      />
    </p>
    <p>Nb de pages total : {{ nbPagesTotal }}</p>
    <p>
      Nb restaurants par page
      <input
        @input="getRestaurantsFromServeur()"
        type="range"
        min="2"
        max="100"
        value="10"
        v-model="pagesize"
      />{{ pagesize }}
    </p>
    <md-button class="md-raised" :disable="page === 0" @click="pagePrecedente()">Précédent</md-button
    >&nbsp;&nbsp;
    <md-button class="md-raised" :disable="page === nbPagesTotal" @click="pageSuivante()">
      Suivant
    </md-button>
    
    <md-table v-model="restaurants">
        <md-table-row  
        slot="md-table-row" 
        slot-scope="{ item, index }"
        :style="{backgroundColor:getColor(index)}" 
        :class="{bordureRouge:(index === 2)}"
      >
          <md-table-cell md-label="Name" md-sort-by="name">{{ item.name }}</md-table-cell>
          <md-table-cell md-label="Cuisine" md-sort-by="cuisine">{{ item.cuisine }}</md-table-cell>
          <md-table-cell md-label="Ville" md-sort-by="ville">{{item.borough}} </md-table-cell>
          <md-table-cell md-label="Action"><router-link :to="'/restaurant/'+ item._id">[Detail d'un restaurant]</router-link></md-table-cell>
        </md-table-row>
    </md-table>
    <md-dialog-alert
      :md-active="AucunRestaurant"
      @click="RechercheNaps()"
      md-title="Post created!"
      md-content="Le restaurant recherché n'existe pas" />



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
  name: "ListeDesRestaurants",
  data: function () {
    return {
      restaurants: [],
      nom: "",
      cuisine: "",
      nbRestaurantsTotal: 0,
      page: 0,
      pagesize: 10,
      nbPagesTotal: 0,
      msg: "",
      nomRestauRecherche: "",
      AucunRestaurant: false,
    };
  },
  mounted() {
    console.log("AVANT AFFICHAGE");
    this.getRestaurantsFromServeur();
  },
  methods: {
    pagePrecedente() {
      if (this.page === 0) return;
      this.page--;
      this.getRestaurantsFromServeur();
    },
    pageSuivante() {
      if (this.page === this.dernierePage) return;
      this.page++;
      this.getRestaurantsFromServeur();
    },
    getRestaurantsFromServeur() {
      let url = "http://localhost:8080/api/restaurants?";
      url += "page=" + this.page;
      url += "&pagesize=" + this.pagesize;
      url += "&name=" + this.nomRestauRecherche;

      fetch(url)
        .then((responseJSON) => {
          responseJSON.json().then((resJS) => {
            // Maintenant res est un vrai objet JavaScript
            this.restaurants = resJS.data;
            this.nbRestaurantsTotal = resJS.count;
            this.AucunRestaurant = false;
            if(this.nbRestaurantsTotal === 0){
              this.AucunRestaurant = true;
            }
            this.nbPagesTotal = Math.round(
              this.nbRestaurantsTotal / this.pagesize
            );
          });
        })
        .catch(function (err) {
          console.log(err);
        });
    },
    chercherRestaurants: _.debounce(function () {
      this.getRestaurantsFromServeur();
    }, 300),
    supprimerRestaurant(r) {
      let url = "http://localhost:8080/api/restaurants/" + r._id;

      fetch(url, {
        method: "DELETE",
      })
        .then((responseJSON) => {
          responseJSON.json().then((resJS) => {
            // Maintenant res est un vrai objet JavaScript
            console.log(resJS.msg);
            this.msg = resJS.msg;
            // On rafraichit la vue
            this.getRestaurantsFromServeur();
          });
        })
        .catch(function (err) {
          console.log(err);
        });
    },
    ajouterRestaurant(event) {
      // Récupération du formulaire. Pas besoin de document.querySelector
      // ou document.getElementById puisque c'est le formulaire qui a généré
      // l'événement
      let form = event.target;

      // Récupération des valeurs des champs du formulaire
      // en prévision d'un envoi multipart en ajax/fetch
      let donneesFormulaire = new FormData(form);

      let url = "http://localhost:8080/api/restaurants";

      fetch(url, {
        method: "POST",
        body: donneesFormulaire,
      })
        .then((responseJSON) => {
          responseJSON.json().then((resJS) => {
            // Maintenant res est un vrai objet JavaScript
            console.log(resJS.msg);
            this.msg = resJS.msg;
            // On rafraichit la vue
            this.getRestaurantsFromServeur();
          });
        })
        .catch(function (err) {
          console.log(err);
        });

      this.nom = "";
      this.cuisine = "";
    },
    getColor(index) {
      return index % 2 ? "lightBlue" : "pink";
    },
    RechercheNaps(){
      this.AucunRestaurant = false;
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
h1 {
  background-color: yellow;
}

.ajouter{
  justify-content: center ;
}
</style>
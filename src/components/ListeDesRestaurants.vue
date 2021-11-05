<template>
  <div>    
<section class="menu">

  <md-menu md-size="small">
      <md-button md-menu-trigger>Master 1 MIAGE</md-button>
    </md-menu>

    <md-menu md-size="medium">
      <md-button md-menu-trigger>DripAdvisor</md-button>
    </md-menu>

    <md-menu md-size="big">
     <md-button md-menu-trigger>restaurant</md-button>
    </md-menu>

    <md-menu md-size="huge">
      <md-button md-menu-trigger>BENALLAL</md-button>
    </md-menu>

    <md-menu md-size="auto">
      <md-button md-menu-trigger>MIGEAT</md-button>
    </md-menu>
</section>


   <div class="container-fluid" style="width: 100%; height: 500px; background-attachment: fixed; background-image: url('pexels.jpg');background-size: 100% 100%; background-repeat: no-repeat;background-position:  cover ;overflow: hidden;">
    <section class="titre">
    <p class="style" >Trouvez le restaurant parfait</p>
    <md-field class="text-center">
    <md-icon>search</md-icon>
      <label>nom du restaurant</label>
      <md-input v-model="nomRestauRecherche"
       @input="chercherRestaurants"></md-input>
    </md-field>
    </section>
   </div>

 
    

   <section class="tab">
    <p>
      Nb restaurants par page <br/> 
      <input 
        class="custom-slider custom-slider-bullet"
        @input="getRestaurantsFromServeur()"
        type="range"
        min="2"
        max="100"
        value="10"
        v-model="pagesize"
      />{{ pagesize }}
    </p>
    
    <md-table v-model="restaurants">
        <md-table-row  
        slot="md-table-row" 
        slot-scope="{ item, index }"  
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
      md-content="Le restaurant recherché n'existe pas"
       />
    </section>

<section class="boubou">
 <md-button class="md-raised" :disable="page === 0" @click="pagePrecedente()">Précédent</md-button
    >&nbsp;&nbsp;
    <md-button class="md-raised" :disable="page === nbPagesTotal" @click="pageSuivante()">
      Suivant
    </md-button>
</section>

  <section class="pop">
  <p>Nombre de restaurants : {{ nbRestaurantsTotal }}</p>
  <p>Nb de pages total : {{ nbPagesTotal }}</p>
  </section>
  <form class="ajouter" v-on:submit.prevent="ajouterRestaurant(event)">
        <md-card class="mdcardajout">
          <md-card-header>
            <div class="md-title">Ajout de restaurant</div>
          </md-card-header>

          <md-field>
            <label> Nom : </label>
            <md-input name="name" type="text" required v-model="nom" />
          </md-field>
          <md-field>
            <label> Cuisine : </label>
            <md-input name="cuisine" type="text" required v-model="cuisine" />
          </md-field>

          <md-button type = submit() >Ajouter</md-button>
        </md-card>
      </form>
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
      event:"",
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


.ajouter{
  justify-content: center ;
}

.md-field {
    
   border: white ;
  border-radius:30px;
  background: white;
  }

.md-input{
  
}

.menu{
  margin-left:59%
}

.tab{
   margin: auto;
   margin-top:5%;
   width:60%;
   border: solid ;
}

.titre{
margin-top: 15%;
font-size:3em;
color:white;

}

.style{
text-align: center;
font-family : Comic Sans MS, Comic Sans, cursive;
}

.boubou{
  margin-left:65%
}

.info{
  border : solid;
  width:18%
}


/*==============================*/
/* le curseur                   */
/*==============================*/
input[type=range].custom-slider.custom-slider-bullet::-webkit-slider-thumb {
  width: 2em;
  padding: 0.25em;                                              /* largeur du bord */
  border: 1px solid #888;
  border-radius: 50%;
  box-shadow: 0 0 .5em #FFF inset;
  background: linear-gradient(#888, #FFF) content-box,          /* le centre */
              linear-gradient(-90deg, #888, #DDD) border-box;   /* le bord */
}


</style>
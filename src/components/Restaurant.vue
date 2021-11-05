<template>
  <div class="back"  style="background-color:#F2F2F2;">  
  <section class="menu">

  <md-menu md-size="small">
      <md-button md-menu-trigger><router-link to="/" style="color:red;">Home</router-link></md-button>
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
  
<section class = "barre"  style=" box-shadow: 8px 8px 12px #aaa;background-color:white"> 
   <h1> Nom :  {{restaurant.name}}  <span style="font-size: 12px;" class="gras"><md-icon style="color: red;">check</md-icon>Page attribuée </span>  </h1>  
    <p><md-icon style="color: red;"><span class="material-icons">
perm_identity
</span></md-icon>Détail du restaurant qui a pour id : <span class="gras">{{id}} </span> <md-icon style="color: red;">location_city</md-icon> Ville :  <span class="gras">  {{restaurant.borough}}  </span><md-icon style="color: red;">room</md-icon>
 adresse : <span class="gras"> {{restaurant.address.street}} </span> <md-icon style="color: red;">store</md-icon>
                Code postale : <span class="gras">{{restaurant.address.zipcode}}</span></p> <br>
    </section> 


<section class="drogua" style="margin-top:50px;  background-color:white; box-shadow: 8px 8px 12px #aaa; margin-bottom:3%; " > 

<imagePorfolio  /> 

</section> 
 
      <div class="container"  >
       
        <div class="row">
          


          <div class="col-sm" >
            <div class="testimonial-2" style="background-color:white; box-shadow: 8px 8px 12px #aaa;">
              <div class="d-flex v-card align-items-center mb-4">
                <h1 style="text-align:center;"> Note</h1>
              </div>
              <blockquote>
              <br>
              <br>
                <p>Les avis et les notes sont les moyennes de tous les internautes qui n'ont pas eu la flemme de noter le resto</p> <br>
              <md-table>
      <md-table-row>
        <md-table-head>Avis</md-table-head>
        <md-table-head>Grade</md-table-head>
        <md-table-head>Score</md-table-head>
        
      </md-table-row>

      <md-table-row>
        <md-table-cell>1</md-table-cell>
        <md-table-cell>{{restaurant.grades[0].grade }}</md-table-cell>
        <md-table-cell>{{restaurant.grades[0].score }}</md-table-cell>
      
      </md-table-row>

      <md-table-row>
        <md-table-cell>2</md-table-cell>
        <md-table-cell>{{restaurant.grades[0].grade }}</md-table-cell>
        <md-table-cell>{{restaurant.grades[1].score }}</md-table-cell>
       
      </md-table-row>

      <md-table-row>
        <md-table-cell>3</md-table-cell>
        <md-table-cell>{{restaurant.grades[0].grade }}</md-table-cell>
        <md-table-cell>{{restaurant.grades[2].score }}</md-table-cell>
        
      </md-table-row>
    </md-table><br><br><br><br><br>
              
               
              </blockquote>
              
            </div>
          </div>
          
          <div class="col-sm" >
            <div class="testimonial-2" style="background-color:white; box-shadow: 8px 8px 12px #aaa;">
              <div class="d-flex v-card align-items-center mb-4">
 <imageCarte/>            
    
              </div>
              <blockquote>
              </blockquote>
              
            </div>
          </div>
          <div class="col-sm" >
            <div class="testimonial-2" style="background-color:white; box-shadow: 8px 8px 12px #aaa; ">
              <div class="d-flex v-card align-items-center mb-4" >
               <h2 style="font-weight:bold; ">  <md-icon>map</md-icon>Coordonées : </h2>
              </div>
              <blockquote class="toka" style="font-size:130%; font-weight:bold; ">
                <Map ref="map"/> <br> 
                   <md-icon>room</md-icon>
 adresse :  {{restaurant.address.street}}<br> <br>
 
                <md-icon>store</md-icon>
                Code postale : {{restaurant.address.zipcode}}<br> <br>

              <md-icon >home</md-icon>  building : {{restaurant.address.building}} 
              </blockquote>
              
            </div>
          </div>
        </div>
      </div>
    
 

    
  </div>
</template>

<script>
import Map from './Map.vue'
import imageCarte from './CarteDesPlats.vue'
import imagePorfolio from './Images.vue'

export default {
  name: 'Restaurant',
  components: {
      Map,
      imagePorfolio,
      imageCarte,
  },
  props: {

  },
  computed: {
      id() {
          return this.$route.params.id
      }
  },
  data: function() {
      return {
          restaurant:null,
      }
  },
  mounted(){
      console.log("Avant affichage, on pourra faire un fetch...");
      console.log("ID = " + this.id)
      let url="http://localhost:8080/api/restaurants/" + this.id;
      fetch(url)
      .then(reponse => {
          return reponse.json();
      }).then(data => {
          this.restaurant = data.restaurant;
          console.log(this.restaurant.address.coord)
        this.$nextTick(function(){
          this.MAJCenter(this.restaurant.address.coord) 
          });      
      })
  },
  methods:{
  
    MAJCenter(center){
      var tmp = center[1]
      center[1]=center[0]
      center[0] = tmp;
        this.$refs.map.centerUpdated(center) 
        this.$refs.map.Setmarker(center)
    }
    
  }
}
</script>

<style >
.gras{
  font-weight:bold;
}
.site-section bg-light{
  background-color:#F2F2F2;
}
.img{
  margin : auto;
  
}

.drogua{
  
  margin : auto;
 
  width : 88%;
}

.md-color{
  color:red;
}
.menu{
  margin-left:59%
}
</style>
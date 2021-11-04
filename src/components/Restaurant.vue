<template>
  <div class="back">  
  
<section class = "barre"> 
   <h1> Nom :  {{restaurant.name}} </h1>  
    <p>Détail du restaurant qui a pour id : {{id}}   Ville :  {{restaurant.borough}} {{restaurant.grades}} </p>
     
<section > 

<imagePorfolio/> 
  </section > 

</section> 
  <div class="site-section bg-light" >
      <div class="container" style="background-color:#F2F2F2;">
        <div class="row justify-content-center text-center mb-5">
          
        </div>
        <div class="row">
          


          <div class="col-lg-4 mb-4 mb-lg-0" >
            <div class="testimonial-2" style="background-color:white;">
              <div class="d-flex v-card align-items-center mb-4">
                <span> Note</span>
              </div>
              <blockquote>
              <br>
              <br>
                <p>Les avis et les notes sont les moyennes de tous les internautes qui n'ont pas eu la flemme de noter le resto</p> <br>
                <p>Grade </p>
                <p>Score</p>
              </blockquote>
              
            </div>
          </div>
          <div class="col-lg-4 mb-4 mb-lg-0" >
            <div class="testimonial-2" style="background-color:white;">
              <div class="d-flex v-card align-items-center mb-4">
 <imageCarte/>            
    
              </div>
              <blockquote>
              </blockquote>
              
            </div>
          </div>
          <div class="col-lg-4 mb-4 mb-lg-0" >
            <div class="testimonial-2" style="background-color:white;">
              <div class="d-flex v-card align-items-center mb-4">
                Emplacement et coordonées
                <span></span>
              </div>
              <blockquote>
                <Map ref="map"/> <br> <br>
                adresse : {{restaurant.address.building}}  {{restaurant.address.street}}<br>
                tieks : <br>
                encore un bails : 
              </blockquote>
              
            </div>
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
.img{
  margin : auto;
}
</style>
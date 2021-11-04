<template>
<div>
    <section>

        <h2 id="SalleResto">{{titre}}</h2>
            <div class="containerImage">
            
            
  <img height="300px" width="400px" v-bind:src="selectedImage"   />

        </div>
        
    </section>
</div>
</template>

<script >

export default {
  name: 'SalleResto',
  props: {
    msg: String
  },

    data(){

        return{
            titre :"Salle du Restaurant",
           images:[
                'https://www.longemallecollection.com/cache/7/9/6/c/0/796c0091371d463d35b74a8b95f66bf1418477e5.jpeg',
                'https://libanais-montrouge.fr/wp-content/uploads/2019/07/salle-libanais-montrouge-4.jpg',
                'https://media-cdn.tripadvisor.com/media/photo-s/0e/64/70/17/la-salle.jpg',
                'https://res.cloudinary.com/tf-lab/image/upload/w_600,h_337,c_fill,g_auto:subject,q_auto,f_auto/restaurant/29e32350-506d-491f-ba77-9d6ff277d738/9db9ea94-95b3-4cce-9b08-a1c16778220e.jpg',
                'https://res.cloudinary.com/tf-lab/image/upload/w_600,h_337,c_fill,g_auto:subject,q_auto,f_auto/restaurant/f1a33db0-902a-4806-b2e8-3e115f98a22b/9ce5ebc8-2d3d-4fa7-8313-84a418f086e1.jpg',
            ],
        }

        
    },

  created () {
    const idx = Math.floor(Math.random() * this.images.length);
    this.selectedImage = this.images[idx]
  }
}
</script>
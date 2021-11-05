<template>
<div>

    <section>
      <h3>  <MenuT/> </h3>
    </section>


<nav class="nav">
<button type="button" class="btn btn-danger" @click="showCart = !showCart">Panier 
        <i class="fas fa-shopping-cart"></i>
      </button> 
    <p >Mon Panier : Prix {{this.price}}€,  </p> 
    <div > 
    
      <span> Quantité : {{ totalQuantity }}</span>
      <div v-if="showCart">
        <ul >
          <li
            v-for="product in cart"
            :key="product.id"
          >
            {{ product.name }} ({{ product.quantity }}, Prix à l'unité : {{product.prix}}€)
          </li>
        </ul>
      </div>
    </div>
  </nav>
<section class="products"><br>
    <div v-for="product in this.Prod.Type" :key="product.id" class="product">
     
      <img
        :src="require(`@/assets/${product.num}/${product.cheminImage}.${product.extension}`)" 
                   class="MenuRepas" :alt="product.altTexte"
        
      >
      <p >{{ product.description }}, Prix : {{product.prix}}€</p>
      <div class="cart">
        <button
          @click="updateCart(product, 'subtract')">
          -
        </button>
        <span >{{ product.quantity }}</span> 
        <button
          @click="updateCart(product, 'add')"          
        >
          +
        </button>
      </div>
    </div>
  </section>  

  <br>
</div>
</template>

<script>
import MenuT from './Menu.vue'


export default {
  name: 'carte',
  components: {
      MenuT,
  },
  props: {
     
    msg: String
  },
    data(){
        return{
           titre:'Menu',
            icon:"1",
            monIndex : 0,
            price : 0,
            
            LeMenu : [{
                nom:"Italien",
                Type : [
                    {
                        num: "1",
                        numero:1,
                        altTexte:"Salade",
                        cheminImage:"1",
                        extension:"jpeg",
                        description:"Entrée : Salade made in Italy",
                        prix: 8,
                    },
                    {
                        num: "1",
                        numero:2,
                        altTexte:"Pasta",
                        cheminImage:"2",
                        extension:"jpg",
                        description:"Plat : Pate faites maison à l'Italienne",
                        prix:16,
                    },{
                        num: "1",
                        numero:3,
                        altTexte:"Tiramisu",
                        cheminImage:"3",
                        extension:"jpg",
                        description:"Déssert : Tiramisu Milanais",
                        prix: 7,
                    }
                ]},{
                    nom:"Libanais",
                Type : [
                    {
                        num: "2",
                        numero:1,
                        altTexte:"Salade",
                        cheminImage:"1",
                        extension:"jpg",
                        description:"Entrée : Taboulé à la libanaise",
                        prix : 10,
                    },
                    {
                        num: "2",
                        numero:2,
                        altTexte:"Plat",
                        cheminImage:"2",
                        extension:"jpeg",
                        description:"Plat : Tapas à la Libanaise ",
                        prix : 19,
                    },{
                        numero:3,
                        num: "2",
                        altTexte:"Déssert",
                        cheminImage:"3",
                        extension:"jpg",
                        description:"Déssert : Douceur d'Orient",
                        prix : 11,
                    }
                ],}
            ],
        
        LeProduit : [{
                Type: [
                {
                num: "1",
                id: 1,
                name: 'Salade',
                description:"Entrée : Salade made in Italy",
                quantity: 0,
                cheminImage:"1",
                extension:"jpeg",
                altTexte:"Salade",
                prix : 8,
                },
                {
                    num: "1",
                id: 2,
                name: 'Pasta',
                description:"Plat : Pate faites maison à l'Italienne",
                quantity: 0,
                cheminImage:"2",
                extension:"jpg",
                altTexte:"Pasta",
                prix : 16,
                },
                {
                    num: "1",
                id: 3,
                name: 'Tiramisu',
                description:"Déssert : Tiramisu Milanais",
                quantity: 0,
                cheminImage:"3",
                extension:"jpg",
                altTexte:"Tiramisu",
                prix : 7,
                }
            ]},
            
            {
          Type: [
              {
                 num: "2",
                id: 1,
                name: 'Salade',
                description:"Entrée : Taboulé à la libanaise",
                quantity: 0,
                cheminImage:"1",
                extension:"jpg",
                altTexte:"Entrée : Taboulé à la libanaise",
                prix : 10,
        },
        {
            num: "2",
          id: 2,
          name: 'Assortiment de plat',
          description:"Plat : Tapas à la Libanaise",
          quantity: 0,
          cheminImage:"2",
          extension:"jpeg",
          altTexte:"Plat : Tapas à la Libanaise",
          prix : 19,
        },
        {
            num: "2",
          id: 3,
          name: 'Gateaux',
          description:"Déssert : Douceur d'Orient",
          quantity: 0,
          cheminImage:"3",
          extension:"jpg",
          altTexte:"Déssert : Douceur d'Orient",
          prix : 11,
        }
        ]

      }],
      showCart: false
        }
    },
    
    created () {
    const index = Math.floor(Math.random() * this.LeMenu.length);
    this.menuChef = this.LeMenu[index] ;
            

    this.Prod = this.LeProduit[index]
    console.log("VAL INDEX"+index)
    console.log("VAL INDEX"+this.Prod.Type[index].description)
    this.ind=index;
    this.indexString = String(index +1);
  },
  methods: {
      updateCart(product, updateType) {  
              
    
        for (let i = 0; i < 3; i++) {
console.log("Prix "+this.price)
           // console.log("VAL TYPE"+this.Prod[this.ind].Type[i].quantity)
          if (this.Prod.Type[i].id === product.id) {
            if (updateType === 'subtract') {
              if (this.Prod.Type[i].quantity !== 0) {
                this.Prod.Type[i].quantity--;
                this.price=this.price-this.Prod.Type[i].prix;
              }
            } else {
              this.Prod.Type[i].quantity++;
              this.price=this.price+this.Prod.Type[i].prix;
            }
            
            break;
          }
        }
      }
    },
    computed: {
        cart() {
        return this.Prod.Type.filter(product => product.quantity > 0);
        },
        totalQuantity() {
        return this.Prod.Type.reduce(
            (total, product) => total + product.quantity,
            0
        );
        }
  },
}
</script>

<style>
.MenuRepas{
  height:60px;
  width:80px;
}
</style>
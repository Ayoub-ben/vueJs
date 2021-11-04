<template>
<div>

    <section>

        <h2 id="porfolio">{{titre}}</h2>
        <MenuT/>
        
        <ul>
        <div v-for="menu in this.LeMenu[this.ind].Type" :key="menu.numero" class="image">
           
           
            <li>
           
            <div class="containerImage">
                <img :src="require(`@/assets/${menu.num}/${menu.cheminImage}.${menu.extension}`)" height="100px"
                    width="150px" class="MenuRepas" :alt="menu.altTexte">
                    <div class="overlay">
                        <div class="text">
                        <p>{{menu.description}}</p>

                        </div>
                    </div>
                
            </div>
        </li>


        </div></ul>
        
    </section>




<nav class="nav">
    <h2 class="nav__header">Mon Panier : Prix {{this.price}}€</h2>
    <div class="nav__cart">
      <button @click="showCart = !showCart">
        <i class="fas fa-shopping-cart"></i>
      </button>
      <span class="total-quantity">{{ totalQuantity }}</span>
      <div v-if="showCart" class="cart-dropdown">
        <ul class="cart-dropdown__list">
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
<section class="products">
    <div v-for="product in this.Prod.Type" :key="product.id" class="product">
      <h3 class="product__header">{{ product.name }}</h3>
      <img
        :src="require(`@/assets/${product.num}/${product.cheminImage}.${product.extension}`)" height="100px"
                    width="150px" class="MenuRepas" :alt="product.altTexte"
        
      >
      <p class="product__description">{{ product.description }}, Prix : {{product.prix}}€</p>
      <div class="cart">
        <button
          @click="updateCart(product, 'subtract')"
          class="cart__button"
        >
          -
        </button>
        <span class="cart__quantity">{{ product.quantity }}</span>
        <button
          @click="updateCart(product, 'add')"
          class="cart__button"
        >
          +
        </button>
      </div>
    </div>
  </section>  

  
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
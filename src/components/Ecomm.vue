<template>
  <div id="ecomm">
  
  <!--
    https://vuejs.org/v2/guide/list.html#Mapping-an-Array-to-Elements-with-v-for
  -->
  <nav class="nav">
    <h2 class="nav__header">Products</h2>
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
            {{ product.name }} ({{ product.quantity }})
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <!--
    https://vuejs.org/v2/guide/list.html#Mapping-an-Array-to-Elements-with-v-for
  -->
  <!-- https://vuejs.org/v2/guide/syntax.html#v-on-Shorthand -->
  <section class="products">
    <div v-for="product in products" :key="product.id" class="product">
      <h3 class="product__header">{{ product.name }}</h3>
      <img
        src="https://via.placeholder.com/150"
        :alt="product.name"
        class="product__image"
      >
      <p class="product__description">{{ product.description }}</p>
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
export default {
  name: 'Ecomm',
  data() {
    return {
      
      showCart: false
    };
  },
  
  // https://vuejs.org/v2/guide/computed.html
  computed: {
    cart() {menu
      return this.menu.filter(menu => product.quantity > 0);
    },
    totalQuantity() {
      return this.menu.reduce(
        (total, menu) => total + menu.quantity,
        0
      );
    }
  },
    
    // https://vuejs.org/v2/guide/events.html#Methods-in-Inline-Handlers
    methods: {
      updateCart(menu, updateType) {      
        for (let i = 0; i < this.menu.length; i++) {
          if (this.menu[i].id === product.id) {
            if (updateType === 'subtract') {
              if (this.menu[i].quantity !== 0) {
                this.menu[i].quantity--;
              }
            } else {
              this.menu[i].quantity++;
            }
            
            break;
          }
        }
      }
    }
 
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

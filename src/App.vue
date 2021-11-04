<template>
  <div id="app">
  
 <Ecomm/>
    <router-link to="/">[Home]</router-link>
    <router-link to="/restaurant"></router-link>
    <router-view></router-view>
    
  </div>
</template>

<script>
import Vue from 'vue'
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'

// Import Bootstrap an BootstrapVue CSS files (order is important)
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
import Ecomm from './components/Ecomm.vue'
// Make BootstrapVue available throughout your project
Vue.use(BootstrapVue)
// Optionally install the BootstrapVue icon components plugin
Vue.use(IconsPlugin)
export default {
  name: 'App',
  components: {
    Ecomm
  }
}
</script>

<style>
</style>

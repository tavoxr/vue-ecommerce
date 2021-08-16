<template>
  <div id="wrapper">
    <Navbar />
  <section class="section">
    <router-view/>
  </section>
  <footer class="footer">
    <p class="has-text-centered">Copyright (c) 2021</p>
  </footer>
  </div>
</template>


<script>
import Navbar from './components/Navbar.vue'

export default {
  name:'App',
  components:{
    Navbar,
  },
  data(){
    return{
      
      cart:{
        items: [],
      }
    }
  },
    mounted(){
      this.cart = this.$store.state.cart
    },
    beforeCreate(){
      this.$store.commit('initializeStore')
    },
   
   
}


</script>
<style lang="scss">
@import '../node_modules/bulma';
</style>

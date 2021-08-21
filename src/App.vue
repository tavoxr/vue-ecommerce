<template>
  <div id="wrapper">
    <Navbar />
    <LoadingBar/>  
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
import LoadingBar from './components/LoadingBar.vue'
import axios from 'axios'

export default {
  name:'App',
  components:{
    Navbar,
    LoadingBar
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

      const token = this.$store.state.token

      if(token){
        axios.defaults.headers.common['Authorization'] = "Token" + token
      }else{
        axios.defaults.headers.common['Authorization'] = ""
      }
    },
   
   
}


</script>
<style lang="scss">
@import '../node_modules/bulma';



</style>

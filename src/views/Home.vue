<template>
  <div class="home">
    <section class="hero is-medium is-dark mb-6">
    <div class="hero-body has-text-centered"> 
      <p class="title mb-6">
        Welcome to MyEcomm
        </p>
         <p class="subtitle">
        The best store online
        </p>
    </div>
    </section>

    <div class="columns is-multiline">
      <div class="column is-12"> 
          <h2 class="is-size-2 has-text-centered">Latest products</h2>
      </div>
      <div class="column is-3" 
           v-for="product in latestProducts"
           v-bind:key="product.id"
           >
     
          <Product  :product="product" />

      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Product from '../components/Product.vue'
import axios from 'axios'

export default {
  name: 'Home',
  components: {
    Product
  },
  data(){
    return {
      latestProducts : []
      }
  },
  mounted(){
    this.getLatestProducts()
    document.title =  'Home | Ecomm'

  },
  methods:{
    async getLatestProducts(){

      try{
        this.$store.commit('setIsLoading', true)
        const response = await axios.get('/api/latest-products/')
        this.$store.commit('setIsLoading', false)

       console.log('latest-products', response.data)
       this.latestProducts = response.data
      }catch(error){
          console.log(error)
      }
    }
  }

}
</script>

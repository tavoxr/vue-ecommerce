<template>
    <div class="page-search">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Search</h1>
                <h2 class="is-size-5 has-text-grey">Search term: "{{query}}"</h2>
            </div>
            <div class="column is-3" 
                 v-for="product in products"
                 v-bind:key="product.id"
                >
                <Product :product="product" />
            </div>

        </div>

    </div>
</template>


<script>
import axios from 'axios';
import Product from '../components/Product.vue';

export default {
    name: 'Search',
    components:{
        Product
    },
    data(){
        return{
            products: [],
            query: ""
        }
    },
    mounted(){
        document.title = "Search | Ecomm"
        let uri = window.location.search.substring(1)
        console.log('uri', uri)
            let params = new URLSearchParams(uri)
            console.log('params',params)
        console.log('paramsgetquery',params.get('query'))

        if(params.get('query')){
            this.query = params.get('query')
            this.performSearch()
        }
    },
    methods:{
         async performSearch(){

            this.$store.commit('setIsLoading', true)
            const response = await axios.post('/api/products/search/',{'query': this.query})
          
            
            console.log('response.data search', response.data)
            this.products =  response.data
            this.$store.commit('setIsLoading', false)
        }
    }


}
</script>
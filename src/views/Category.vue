<template>
    <div class="page-category">
        <div class="columns is-multiline" > 
            <div class="column is-12"> 
                <h2 class="is-size-2 has-text-centered">{{category.name}}</h2>

            </div>
             <div class="column  is-one-third-desktop" 
           v-for="product in category.products"
           v-bind:key="product.id"
           >
     
          <Product  :product="product" />

      </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import {toast} from 'bulma-toast';
import Product from '../components/Product.vue'

export default {
    name: 'Category',
    components:{
        Product
        
    },
    data(){
        return{
            category:{
                products: []
            }
        }
    },
    mounted(){
        this.getCategory()
    },
    watch:{
        $route(to, from){
            if(to.name === 'Category'){
                this.getCategory()
            }
        }
    },
    methods:{
        async getCategory(){
            const category_slug =  this.$route.params.category_slug
            
            try{
                this.$store.commit('setIsLoading', true)
                const response = await axios.get(`/api/products/${category_slug}`)
                this.$store.commit('setIsLoading',  false )

                console.log('category response.data', response.data)
                this.category= response.data

                document.title =  this.category.name + ' | Ecomm'

                
            }catch(error){
                console.log(error)
                
                toast({
                message: "Something went wrong, Please try again.",
                type: "is-danger",
                dismissible: true,
                pauseOnHover: true,
                duration:2000,
                position:"bottom-right"
            })
            }
        }
    }
    
}
</script>
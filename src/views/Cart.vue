<template>
    <div class="page-cart">
        <div class="column is-12">
            <h1 class="title">Cart</h1>
        </div>

        <div class="column is-12-box">
            <div class="table-container">
            <table class="table is-fullwidth" v-if="cartTotalLength">
                <thead>
                    <tr>
                        <th class="has-text-centered">Product</th>
                        <th class="has-text-centered">Price</th>
                        <th class="has-text-centered">Quantity</th>
                        <th class="has-text-centered">Total</th>
                        <th></th>
                    </tr>
                </thead>
                <tbody>
                    <CartItem
                        v-for="item in cart.items"
                        v-bind:key = "item.product.id" 
                        v-bind:initialItem="item"
                        v-on:removeFromCart="removeFromCart"

                    />

                    
                </tbody>
            </table>

            
            <p v-else>You don't have any products in your cart...</p>
            </div>

            <div class="column is-12 box">
                <h2 class="subtitle">Summary</h2>
                <strong>${{ cartTotalPrice.toFixed(2)}}</strong>, {{cartTotalLength}} items
                <hr/>

                <router-link to="/cart/checkout" class="button is-dark">Proceed to checkout</router-link>
            </div>
        </div>
    </div>
</template>


<script>
import axios from 'axios';
import CartItem from '../components/CartItem.vue'

export default {
    name: 'Cart',
    components:{
        CartItem
    },
    data(){
        return{
            cart:{
                items: []
            }
        }
    },
    mounted(){
        this.cart = this.$store.state.cart
    },
    computed:{
        cartTotalLength(){
            return this.cart.items.reduce((accumulator,currentValue)=>{
                return accumulator += currentValue.quantity
            },0)
        },
        cartTotalPrice(){
            return this.cart.items.reduce((accumulator,currentValue)=>{
                return accumulator += currentValue.quantity * currentValue.product.price
                    
            },0)
        }
    },
    methods:{
        removeFromCart(item){
            this.cart.items = this.cart.items.filter(i=> i.product.id !== item.product.id)
        }
      
    }
}
</script>
<template>
     <div class="box">
        <figure class="image mb-4">
          <img v-bind:src="product.get_thumbnail" />
        </figure>
        <h3 class="is-size-4">{{product.name}}</h3>
        <p class="is-size-6 has-text-grey">$ {{product.price}}</p>
        
        View details

      </div> 
</template>

<script>

export default {
    name: 'Product',
    props: {
        product : Object
    }
    
    
}
</script>





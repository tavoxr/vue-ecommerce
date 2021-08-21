<template>
    <nav class="navbar is-dark">
      <div class="navbar-brand">
          <router-link to="/" class="navbar-item">
            <strong>Ecomm</strong>
          </router-link>

          <a role="button"  class="navbar-burger" 
                            aria-label="menu" 
                            aria-expanded="false" 
                            data-target="navbarMenu"
                            
                            >
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
          </a>
      </div> 

      <div id="navbarMenu" class="navbar-menu">
          <div class="navbar-start"> 
              <div class="navbar-item"> 
                  <form method="GET" action="/search"   >
                  <div class="field has-addons"> 
                      <div class="control"> 
                          <input type="text" 
                                 class="input" 
                                 placeholder="What are you looking for?"
                                 name="query"
                                />
                      </div>


                    <div class="control">
                        <button class="button is-success">
                            <span class="icon">
                                <i class="fas fa-search"></i>
                            </span>
                        </button>

                    </div>
                  </div>

                  </form>
              </div>

          </div>
        <div class="navbar-end">
          <router-link to="/summer" class="navbar-item"  >Summer</router-link>
          <router-link to="/winter" class="navbar-item">Winter</router-link>
          
          <div class="navbar-item">
              <div class="buttons">
                <template v-if="$store.state.isAuthenticated">
                  <router-link to="/my-account" class="button is-light" >My account </router-link>    
                </template>
                <template v-else>
                  <router-link to="/log-in"  class="button is-light">Log in</router-link>
                </template>
                <router-link to="/cart"  class="button is-success">
                <span class="icon"><i class="fas fa-shopping-cart"></i></span>
                <span class="">Cart ({{cartTotalLength}})</span>
                </router-link>

              </div>
          </div>
        </div>

      </div>
    </nav>
</template>


<script >
export default {
    name: 'Navbar',
    props:{
        
    },
     computed:{
      cartTotalLength(){
        let totalLength= 0

        for(let i=0; i < this.$store.state.cart.items.length; i++){
          totalLength  += this.$store.state.cart.items[i].quantity
        }
        return totalLength
      }
    },
   
}

document.addEventListener('DOMContentLoaded', () => {

  // Get all "navbar-burger" elements
  const $navbarBurgers = Array.prototype.slice.call(document.querySelectorAll('.navbar-burger'), 0);

  // Check if there are any navbar burgers
  if ($navbarBurgers.length > 0) {

    // Add a click event on each of them
    $navbarBurgers.forEach( el => {
      el.addEventListener('click', () => {

        // Get the target from the "data-target" attribute
        const target = el.dataset.target;
        const $target = document.getElementById(target);

        // Toggle the "is-active" class on both the "navbar-burger" and the "navbar-menu"
        el.classList.toggle('is-active');
        $target.classList.toggle('is-active');

      });
    });
  }

    });
</script>
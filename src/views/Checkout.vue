<template>
    <div class="page-checkout">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Checkout</h1>
            </div>

            <div class="column is-12 box">
            <div class="table-container">
            <table class="table is-fullwidth" >
                <thead>
                    <tr>
                        <th >Product</th>
                        <th >Price</th>
                        <th >Quantity</th>
                        <th >Total</th>
                        <th></th>
                    </tr>
                </thead>
                <tbody>
                     <tr v-for="item in cart.items" 
                        v-bind:key="item.product.id"
                         >
                            <td>{{item.product.name}}</td>
                            <td>{{item.product.price}}</td>                            
                            <td>{{item.quantity}}</td>
                            <td>${{getItemTotal(item).toFixed(2)}}</td>
                    </tr>
                </tbody>
                <tfoot>
                    <tr>
                        <td colspan="2">Total</td>
                        <td>{{cartTotalLength}}</td>
                        <td>${{cartTotalPrice.toFixed(2)}}</td>
                        <td></td>
                    </tr>
                </tfoot>
            </table>
            </div>
            </div>
            <div class="column is-12 box">
                <h2 class="subtitle">Shipping details</h2>
                <p class="has-text-grey mb-4">All fields are required</p>
                <div class="columns is-multiline">
                    <div class="column is-6">
                        <div class="field">
                            <label>First name*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="first_name"/>
                            </div>
                        </div>
                        <div class="field">
                            <label>Last name*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="last_name"/>
                            </div>
                        </div>
                        <div class="field">
                            <label>E-mail*</label>
                            <div class="control">
                                <input type="email" class="input" v-model="email"/>
                            </div>
                        </div>
                        <div class="field">
                            <label>Phone*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="phone"/>
                            </div>
                        </div>
                    </div>
                    <div class="column is-6">
                        <div class="field">
                            <label>Address*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="address"/>
                            </div>
                        </div>
                        <div class="field">
                            <label>Zip code*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="zipcode"/>
                            </div>
                        </div>
                        <div class="field">
                            <label>Place*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="place"/>
                            </div>
                        </div>
                    </div>

                    
                

                </div>
                <div class="notification is-danger mt-4" v-if="errors.length">
                        <p v-for="error in errors" v-bind:key="error">{{error}}</p>
                </div>
                    <hr/>
                    <div class="is-warning mb-3">

                        <p><strong>Note:</strong>This is not a real ecommerce, all products showed in this app are not real.</p>
                        <p>In order to test the payments please follow the instructions below:</p>
                        <p><strong>Credit or Debit Card:</strong> Use any of the following test card numbers, a valid expiration date in the future, 
                            and any random CVC number, to create a successful payment:<br/>
                            Visa	    4242424242424242 <br/>
                            Mastercard	5555555555554444<br/>
                            or you can check other test cards <a id="hereLink" target="_blank" href="https://stripe.com/docs/testing">here</a>
                        </p>

                    </div>
                    <div id="card-element" class="mb-5"></div>

                    <template v-if="cartTotalLength">
                        <hr>
                        <button class="button is-dark" @click="submitForm">Pay with Stripe</button>

                    </template>
            </div>




        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Checkout",
  data() {
    return {
      cart: {
        items: [],
      },
      stripe: {},
      card: {},
      first_name: "",
      last_name: "",
      email: "",
      phone: "",
      address: "",
      zipcode: "",
      place: "",
      errors: [],
    };
  },
  mounted() {
    document.title = "Checkout | Ecomm";
    this.cart = this.$store.state.cart;

    if (this.cartTotalLength > 0) {
      this.stripe = Stripe(
        "pk_test_51JR1v7FDHFihXWeH4fXUgGcrG6r69FGfrN93xoW6D5KZcmSXKWtcpEFbJjrNHc3Pl42r7WXnoPICN28ULHE6SR4N00qOfJMwyN"
      );
      const elements = this.stripe.elements();
      this.card = elements.create("card", { hidePostalCode: true });

      this.card.mount("#card-element");
    }
  },
  methods: {
    getItemTotal(item) {
      return item.quantity * item.product.price;
    },
    submitForm() {
      this.errors = [];

      if (this.first_name === "") {
        this.errors.push("The first name field is missing!");
      }
      if (this.last_name === "") {
        this.errors.push("The last name field is missing!");
      }
      if (this.email === "") {
        this.errors.push("The email  field is missing!");
      }
      if (this.phone === "") {
        this.errors.push("The phone field is missing!");
      }
      if (this.address === "") {
        this.errors.push("The address field is missing!");
      }
      if (this.zipcode === "") {
        this.errors.push("The zipcode field is missing!");
      }
      if (this.place === "") {
        this.errors.push("The place field is missing!");
      }

      if (!this.errors.length) {
        this.$store.commit("setIsLoading", true);
        this.stripe.createToken(this.card).then((result) => {
          if (result.error) {
            this.$store.commit("setIsLoading", false);
            this.errors.push(
              "Something went wrong with Stripe. Please try again"
            );
            console.log(result.error.message);
          } else {
            this.stripeTokenHandler(result.token);
          }
        });
      }
    },
    async stripeTokenHandler(token) {
      const items = [];

      for (let i = 0; i < this.cart.items.length; i++) {
        const item = this.cart.items[i];
        const obj = {
          product: item.product.id,
          quantity: item.quantity,
          price: item.product.price * item.quantity,
        };
        items.push(obj);
      }

      const data = {
        first_name: this.first_name,
        last_name: this.last_name,
        email: this.email,
        address: this.address,
        zipcode: this.zipcode,
        place: this.place,
        phone: this.phone,
        items: items,
        stripe_token: token.id,
      };

      try {
        const response = await axios.post("/api/checkout/", data);

        this.$store.commit("clearCart");
        this.$router.push("/cart/success");
      } catch (error) {
        this.errors.push("Something went wrong, Please try again");
        console.log(error);
      }

      this.$store.commit("setIsLoading", false);
    },
  },
  computed: {
    cartTotalLength() {
      return this.cart.items.reduce((accumulator, currentValue) => {
        return (accumulator += currentValue.quantity);
      }, 0);
    },
    cartTotalPrice() {
      return this.cart.items.reduce((accumulator, currentValue) => {
        return (accumulator +=
          currentValue.quantity * currentValue.product.price);
      }, 0);
    },
  },
};
</script>
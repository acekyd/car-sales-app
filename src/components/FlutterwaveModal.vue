// src/components/FlutterwaveModal.vue

<template>
  <div class="flw">
    <button class="button" @click="makePayment">Book Now</button>
  </div>
</template>

<script>
  export default {
    name: 'FlutterwaveModal',
    props: {
        isProduction: {
          type: Boolean,
          required: false,
          default: false //set to true if you are going live
        },
        email: {
          type: String,
          required: true
        },
        amount: {
          type: Number,
          required: true
        },
        flwKey: {
          type: String,
          required: true
        },
        callback: {
          type: Function,
          required: true,
          default: () => {}
        },
        close: {
          type: Function,
          required: true,
          default: () => {}
        },
        currency: {
          type: String,
          default: "NGN"
        },
        country: {
          type: String,
          default: "NG"
        },
        custom_title: {
          type: String,
          default: ""
        },
        custom_logo: {
          type: String,
          default: ""
        },
        reference: {
          type: String,
          required: true
        },
        payment_method: {
          type: String,
          default: "card,mobilemoney,ussd"
        }
      },
      created() {
        const script = document.createElement("script");
        script.src = !this.isProduction
          ? "https://checkout.flutterwave.com/v3.js"
          : "https://ravemodal-dev.herokuapp.com/v3.js";
        document.getElementsByTagName("head")[0].appendChild(script);
      },
      methods: {
        makePayment() {
          //window.FlutterwaveCheckout
          window.FlutterwaveCheckout({
              public_key: this.flwKey,
              tx_ref: this.reference,
              amount: this.amount,
              currency: this.currency,
              payment_options: this.payment_method,
              customer: {
                email: this.email,
              },
              callback: response => this.callback(response),
              customizations: {
                title: this.custom_title,
                description: "Payment for items in cart",
                logo: this.custom_logo,
              },
            });
          }
        }
      }
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.button {
  background-color: hsl(200, 100%, 70%);
  color: hsl(0, 0%, 15%);
  width: 100px;
  height: 40px;
  font-weight: 800;
  border-radius: 5px;
  opacity: 0.7;
  transition: 0.5s;
}
.button:hover {
  opacity: 1;
}
</style>
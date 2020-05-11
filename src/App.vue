<template>

  <div id="app">
    <header>
      <h2>
        RENT A CAR. CALL 080-RENT-A-CAR
      </h2>
    </header>
    <v-container grid-list-xl>
      <v-layout wrap>
        <v-flex xs4 v-for="car in carResponse" :key="car[0]" mb-2>
          <v-card>
            <v-img :src="car.media.photo_links[0]" aspect-ratio="2"></v-img>
            <v-card-title primary-title>
              <div>
                <h3>{{ car.build.make }} {{ car.build.model }}</h3>
                <div>Year: {{ car.build.year }}</div>
                <div>Type: {{ car.build.vehicle_type }}</div>
                <div>Mileage: {{ car.miles }} miles</div>
                <div>NGN {{ car.price }} / Day</div>
              </div>
            </v-card-title>
            <v-card-actions class="justify-center">
              <flutterwave
                :isProduction="isProduction"
                :email="email"
                :amount="car.price"
                :reference="reference"
                :flw-key="flwKey"
                :callback="callback"
                :close="close"
                :currency="currency"
                :country="country"
                :custom_title="custom.title"
                :custom_logo="custom.logo"
                :payment_method="paymentMethod"
              />
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>



<script>
import axios from 'axios';
import Flutterwave from "./components/FlutterwaveModal.vue";

const carKey = process.env.VUE_APP_CAR_API_KEY;
const flwKey = process.env.VUE_APP_FLUTTERWAVE_TEST_KEY;

  export default {
    name: "app",
    components: {
        Flutterwave
      },
      data() {
        return {
          carResponse: [],
          isProduction: false,
          flwKey: flwKey,
          email: "ugwuraphael@gmail.com",
          amount: "",
          currency: "NGN",
          country: "NG",
          custom: {
            title: "Car Shop",
          },
          paymentMethod: ""
        };
      },
      computed: {
      reference() {
        let text = "";
        let possible =
          "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
        for (let i = 0; i < 10; i++)
          text += possible.charAt(Math.floor(Math.random() * possible.length));
        return text;
      }
    },
    mounted() {
      axios
        .get('https://marketcheck-prod.apigee.net/v1/search?&year=2016&make=toyota&api_key='+carKey+'&Content-Type=application/json')
        .then(response => {
            this.carResponse = response.data.listings;
        })
        .catch(error => {
            console.log(error);
        });
    }
  }
</script>


  <style>
    #app {
      font-family: "Avenir", Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: justify;
      background-color: hsla(0, 0%, 75%, 0.1);
    }
    header {
      margin-bottom: 60px;
      text-align: center;
    }
    </style>
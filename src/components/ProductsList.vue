<template>
  <v-container>
    <v-row class="ma-3 pa-3">
      <v-col>
        <ShoppingCart :cart="cart" @remove-from-cart="removeFromCart" />
      </v-col>
    </v-row>
    <v-row class="ma-3 pa-3">
      <v-col
        v-for="product in products"
        :key="product.id"
        cols="12" sm="4" md="3"
      >
      <v-card elevation="2" class="ma-3 pa-3">
          <v-card-title class="text-h6">{{ product.title }}</v-card-title>
          <v-img
            :src="product.image"
            class="white--text align-end"
            aspect-ratio="1.7"
            :gradient="'to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)'"
          ></v-img>
          <v-card-subtitle class="pb-3">{{ formatCurrency(product.price) }}</v-card-subtitle>
          <v-card-actions>
            <v-btn color="primary" @click="addToCart(product)">Add to Cart</v-btn>
            <v-btn text @click="selectedProduct = product; dialog = true">Details</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <v-dialog v-model="dialog" persistent max-width="600px">
      <template v-slot:default="dialog">
        <v-card shaped>
          <v-toolbar color="deep-purple accent-4" dark>
            <v-toolbar-title>{{ selectedProduct?.title }}</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-btn icon @click="closeDialog(dialog)">
              <v-icon>mdi-close</v-icon>
            </v-btn>
          </v-toolbar>
          <v-img :src="selectedProduct?.image" aspect-ratio="1.5"></v-img>
          <v-card-title>
            {{ selectedProduct?.title }}
          </v-card-title>
          <v-card-text class="product-details">
            <div class="details-category">{{ selectedProduct?.category }}</div>
            <div class="details-description">{{ selectedProduct?.description }}</div>
          </v-card-text>
          <v-card-actions>
            <v-btn color="deep-purple" text @click="closeDialog(dialog)">Close</v-btn>
          </v-card-actions>
        </v-card>
      </template>
    </v-dialog>
  </v-container>
</template>

<script>
import axios from 'axios';
import ShoppingCart from './ShoppingCart.vue';

export default {  
  components: {
    ShoppingCart
  },
  data() {
    return {
      products: [],
      selectedProduct: null,
      cart: [],
      dialog: false,
    };
  },
  created() {
    this.fetchProducts();
  },
  methods: {
    async fetchProducts() {
      try {
        const response = await axios.get('http://localhost:8000/api/products');
        this.products = response.data;
      } catch (error) {
        console.error('Error fetching products:', error);
        this.products = [];
      }
    },
    selectProduct(product) {
      this.selectedProduct = product;
      this.dialog = true;
    },
    addToCart(product) {
      console.log(product);
      this.cart.push(product);
    },
    removeFromCart(index) {
      this.cart.splice(index, 1);
    },
    formatCurrency(value) {
      return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(value);
    },
    closeDialog() {
      this.dialog = false;
    },
  }
};
</script>

<style scoped>
  .product-details {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    padding-left: 16px;
  }

  .details-category {
    font-weight: 500; 
    margin-bottom: 8px;
  }

  .details-description {
    font-size: 0.875rem;
  }
</style>
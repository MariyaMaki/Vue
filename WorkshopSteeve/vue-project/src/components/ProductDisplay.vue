<script>
import ProductDetails from "./ProductDetails.vue";
export default {
  components: {
    ProductDetails,
  },
  props: {
    premium: {
      type: Boolean,
      required: true,
    },
    cart: {
      type: Number,
    },
  },
  data() {
    return {
      product: "Socks",
      description: "Chaussettes douces, chaudes et vertes",
      url: "https://laracasts.com/series/learn-vue-3-step-by-step/episodes/6",
      onSale: true,
      details: ["50% coton", "30% wool", "20% polyester"],
      variants: [
        {
          id: 1,
          color: "green",
          image: "./src/assets/images/socks_green.jpg",
          quantity: 15,
        },
        {
          id: 2,
          color: "blue",
          image: "./src/assets/images/socks_blue.jpg",
          quantity: 0,
        },
      ],
      sizes: ["S", "M", "L", "XL"],
      brand: "Vue mastery",
      selectedVariant: 0,
    };
  },
  methods: {
    addToCart() {
      if (this.variants[this.selectedVariant].quantity > 0) {
        this.$emit("add-to-cart");
        this.variants[this.selectedVariant].quantity -= 1;
      }
    },
    removeToCart() {
      if (this.cart > 0) {
        this.$emit("remove-to-cart");
        this.variants[this.selectedVariant].quantity += 1;
      }
    },
    updateVariant(index) {
      this.selectedVariant = index;
    },
  }, //on met des fonctions qui seront générees grâce à des événements.
  computed: {
    title() {
      return this.brand + " " + this.product;
    },
    image() {
      return this.variants[this.selectedVariant].image;
    },
    inStock() {
      return this.variants[this.selectedVariant].quantity;
    },
    onSaleDisplay() {
      if (this.onSale == true) {
        return this.brand + " " + this.product + " are on sale!";
      } else {
        return this.brand + " " + this.product + " will be on sale soon!";
      }
    },
    shipping() {
      if (this.premium === true) {
        return "Free";
      } else {
        return 4.99;
      }
    },
  }, //un calcul qui se fait automatiquement, pas d'event
};
</script>

<template>
  <div class="product-display">
    <div class="production-container">
      <div class="product-image">
        <a :href="url" target="_blank">
          <img
            v-bind:src="image"
            alt="image de chaussettes"
            :class="{ outOfStockImg: !inStock }"
        /></a>
      </div>

      <div class="product-info">
        <h1>{{ title }}</h1>
        <p>{{ description }}</p>
        <p v-if="inStock">In stock</p>
        <p v-else>Out of stock</p>
        <p>Shipping: {{ shipping }}</p>
        <p>{{ onSaleDisplay }}</p>
        <div class="list">
          <ProductDetails :details="details" />
          <ul>
            <li v-for="size in sizes">{{ size }}</li>
          </ul>
        </div>

        <div class="circle__container">
          <div
            class="color-circle"
            :style="{ backgroundColor: variant.color }"
            v-for="(variant, index) in variants"
            :key="variant.id"
            @mouseover="updateVariant(index)"
          ></div>
        </div>
        <div class="button__container">
          <button
            class="button"
            v-on:click="addToCart"
            :class="{ disabledButton: !inStock }"
            :disabled="!inStock"
          >
            Add to cart
          </button>
          <button
            class="button"
            v-on:click="removeToCart"
            :class="{ disabledButton: cart == 0 }"
            :disabled="cart == 0"
          >
            Remove from cart
          </button>
        </div>
      </div>
      <div class="cart">Cart ({{ cart }})</div>
    </div>
  </div>
</template>

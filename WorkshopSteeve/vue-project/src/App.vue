<script>
import ProductDisplay from "./components/ProductDisplay.vue";
import ReviewForm from "./components/ReviewForm.vue";
import ReviewList from "./components/ReviewList.vue";

export default {
  components: {
    ProductDisplay,
    ReviewForm,
    ReviewList,
  },
  data() {
    return {
      cart: 0,
      premium: true,
      reviews: [],
    };
  },
  methods: {
    updateCart() {
      this.cart += 1;
    },
    removeCart() {
      this.cart -= 1;
    },
    addReview(review) {
      this.reviews.push(review);
    },
  },
};
</script>

<template>
  <div class="nav-bar"></div>
  <ProductDisplay
    :cart="cart"
    :premium="premium"
    @add-to-cart="updateCart"
    @remove-to-cart="removeCart"
  />
  <ReviewList :reviews="reviews" />
  <ReviewForm @review-submitted="addReview" />
</template>

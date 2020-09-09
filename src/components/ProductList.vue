<template>
  <div class="list">
    <ProductCard v-for="product in products" :key="product.id" :product="product" />
  </div>
</template>

<script>
import ProductCard from './ProductCard.vue';

export default {
  components: {
    ProductCard,
  },
  props: {
    products: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style lang="scss">
.list {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
}
</style>

<template>
  <div class="list">
    <ProductCard
      v-for="product in products"
      :key="product.id"
      :product="product"
      @handle-buy="handleBuy"
    />
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
  methods: {
    handleBuy() {
      this.$emit('handle-buy');
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

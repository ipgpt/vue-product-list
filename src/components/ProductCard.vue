<template>
  <div class="card">
    <img class="card__image" :src="product.thumbnailUrl" alt="product" />
    <div class="card__body">
      <p class="card__text">{{product.title}}</p>
      <button
      class="card__button"
      :class="{ 'card__button--in-busket': product.isInBasket }"
      @click="$emit('handle-buy', product.id)"
      >
        <Loader v-if="product.loading" />
        {{ product.loading ? '' : product.isInBasket ? 'В корзине' : 'Купить'}}
      </button>
    </div>
  </div>
</template>

<script>
import Loader from './Loader.vue';

export default {
  components: {
    Loader,
  },
  props: {
    product: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped lang="scss">
.card {
  width: 31%;
  margin: 1%;
  padding: 20px;
  box-sizing: border-box;
  border: 1px solid #ccc;

  &__button {
    height: 35px;
    width: 100%;

    &--in-busket {
      background-color: #87cefa;
    }
  }
}

@media screen and (max-width: 768px) {
  .card {
    width: 48%;
  }
}

@media screen and (max-width: 576px) {
  .card {
    width: 100%;
  }
}
</style>

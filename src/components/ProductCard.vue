<template>
  <div class="card">
    <img class="card__image" :src="product.thumbnailUrl" alt="product" />
    <div class="card__body">
      <p class="card__text">{{product.title}}</p>
      <button class="card__button">Купить</button>
    </div>
  </div>
</template>

<script>
export default {
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

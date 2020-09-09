<template>
  <div id="app">
    <h1>Product page</h1>
    <ProductList :products="products" @handle-buy="handleBuy" />
  </div>
</template>

<script>
import axios from 'axios';
import ProductList from './components/ProductList.vue';

const productsLimit = '?_limit=50';

export default {
  name: 'App',
  components: {
    ProductList,
  },
  data() {
    return {
      products: [],
    };
  },
  methods: {
    handleBuy() {
      axios
        .get('https://jsonplaceholder.typicode.com/posts/1')
        .then((res) => console.log(res))
        .catch((err) => console.log(err));
    },
  },
  mounted() {
    axios
      .get(`https://jsonplaceholder.typicode.com/photos${productsLimit}`)
      .then((res) => {
        this.products = res.data;
      })
      .catch((err) => console.log(err));
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>

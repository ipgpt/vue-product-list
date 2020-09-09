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
    handleBuy(id) {
      this.products = this.products.map((item) => {
        if (item.id === id) {
          return {
            albumId: item.item,
            id: item.id,
            title: item.title,
            thumbnailUrl: item.thumbnailUrl,
            url: item.url,
            isInBasket: item.isInBasket,
            loading: true,
          };
        }
        return item;
      });
      axios
        .get('https://jsonplaceholder.typicode.com/posts/1')
        .then((res) => {
          if (res.status === 200) {
            this.products = this.products.map((item) => {
              if (item.id === id) {
                return {
                  albumId: item.item,
                  id: item.id,
                  title: item.title,
                  thumbnailUrl: item.thumbnailUrl,
                  url: item.url,
                  loading: item.loading,
                  isInBasket: !item.isInBasket,
                };
              }
              return item;
            });
          }
          this.products = this.products.map((item) => {
            if (item.id === id) {
              return {
                albumId: item.item,
                id: item.id,
                title: item.title,
                thumbnailUrl: item.thumbnailUrl,
                url: item.url,
                isInBasket: item.isInBasket,
                loading: false,
              };
            }
            return item;
          });
        })
        .catch((err) => console.log(err));
    },
  },
  mounted() {
    axios
      .get(`https://jsonplaceholder.typicode.com/photos${productsLimit}`)
      .then((res) => {
        this.products = res.data;
        this.products = this.products.map((item) => Object.assign(item, {
          loading: false,
          isInBasket: false,
        }));
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

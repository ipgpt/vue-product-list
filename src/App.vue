<template>
  <div id="app">
    <h1>Product page</h1>
    <ProductList :products="products" @handle-buy="handleBuy" />
    <footer>
      <Loader v-if="showLoader" />
      <div ref="infiniteScrollTrigger" id="scroll-trigger"></div>
    </footer>
  </div>
</template>

<script>
import axios from 'axios';
import ProductList from './components/ProductList.vue';
import Loader from './components/Loader.vue';

export default {
  name: 'App',
  components: {
    ProductList,
    Loader,
  },
  data() {
    return {
      products: [],
      savedProducts: [],
      currentPage: 1,
      maxPerPage: 50,
      totalResult: 100,
      showLoader: false,
    };
  },
  methods: {
    scrollTrigger() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (
            entry.intersectionRatio > 0
            && this.currentPage < this.pageCount
          ) {
            this.showLoader = true;
            axios
              .get('https://jsonplaceholder.typicode.com/photos')
              .then((res) => {
                const { length } = this.products;
                const newProducts = res.data.slice(
                  length,
                  length + this.maxPerPage,
                );
                const newProductsUpdated = newProducts.map((item) => {
                  const isSavedItem = Boolean(
                    this.savedProducts.find((obj) => obj.id === item.id),
                  );
                  return { ...item, loading: false, isInBasket: isSavedItem };
                });
                this.products = this.products.concat(newProductsUpdated);
                this.currentPage += 1;
                this.showLoader = false;
              })
              .catch((err) => console.log(err));
          }
        });
      });
      observer.observe(this.$refs.infiniteScrollTrigger);
    },
    saveProducts(id) {
      let product = this.products.find((item) => item.id === id);
      if (product) {
        product = { id: product.id };
        const isInStorage = this.savedProducts.find((item) => item.id === id);
        if (isInStorage) {
          this.savedProducts = this.savedProducts.filter(
            (item) => item.id !== id,
          );
        } else {
          this.savedProducts.push(product);
        }
        const parsedData = JSON.stringify(this.savedProducts);
        localStorage.setItem('products', parsedData);
      }
    },
    handleBuy(id) {
      this.products = this.products.map((item) => {
        if (item.id === id) {
          return { ...item, loading: true };
        }
        return item;
      });
      axios
        .get('https://jsonplaceholder.typicode.com/posts/1')
        .then((res) => {
          if (res.status === 200) {
            this.products = this.products.map((item) => {
              if (item.id === id) {
                return { ...item, isInBasket: !item.isInBasket };
              }
              return item;
            });
          }
          this.products = this.products.map((item) => {
            if (item.id === id) {
              return { ...item, loading: false };
            }
            return item;
          });
          this.saveProducts(id);
        })
        .catch((err) => console.log(err));
    },
  },
  computed: {
    pageCount() {
      return Math.ceil(this.totalResult / this.maxPerPage);
    },
  },
  mounted() {
    if (localStorage.getItem('products')) {
      try {
        this.savedProducts = JSON.parse(localStorage.getItem('products'));
      } catch (e) {
        localStorage.removeItem('products');
      }
    }
    axios
      .get('https://jsonplaceholder.typicode.com/photos')
      .then((res) => {
        this.totalResult = res.data.length;
      })
      .catch((err) => console.log(err));
    this.scrollTrigger();
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
footer {
  height: 100px;
}
#scroll-trigger {
  height: 50px;
}
</style>

<template>
  <div :class="namakelas">
    <b-container class="kampret d-flex align-items-center justify-content-center" style="min-height: 100vh">
      <b-row>
        <b-col md="6">
          <img v-if="currentFilteredProduct" :src="currentFilteredProduct.image" class="img-fluid" style="height: 300px" />
          <div v-else class="no-product-img">No Image</div>
        </b-col>
        <b-col md="6">
          <h2 v-if="currentFilteredProduct" style="color: #720060"></h2>
          <h2 v-if="iswomenClothing" :style="titleStyle">{{ currentFilteredProduct.title }}</h2>
          <h2 v-else-if="ismenClothing" :style="titleStyle">{{ currentFilteredProduct.title }}</h2>
          <h2 v-else>No matching Products found</h2>
          <div class="row">
            <b-col md="6">
              <p v-if="currentFilteredProduct">{{ currentFilteredProduct.category }}</p>
            </b-col>
            <b-col md="6">
              <p>3/5</p>
            </b-col>
            <b-col>
              <p v-if="currentFilteredProduct">{{ currentFilteredProduct.description }}</p>
            </b-col>
          </div>
          <br />
          <br />
          <b-col>
            <p v-if="currentFilteredProduct"></p>
            <p v-if="iswomenClothing" :style="priceStyle">${{ currentFilteredProduct.price }}</p>
            <p v-else-if="ismenClothing" :style="priceStyle">${{ currentFilteredProduct.price }}</p>
          </b-col>

          <b-row class="d-flex align-items-center">
            <div class="d-flex justify-content-between">
              <b-col md="12">
                <button v-if="iswomenClothing || ismenClothing" :style="button1Style">Buy Product</button>
                <button v-if="iswomenClothing || ismenClothing" @click="nextProduct" :style="button2Style">Next Product</button>
              </b-col>
            </div>
          </b-row>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      align: 'center',
      bacgroundcolor: 'red',
      products: [],
      currentProductIndex: 0,
      marginTop: 5,
      medium: 6,
      namakelas: 'bungkus',
    };
  },
  computed: {
    currentProduct() {
      return this.products[this.currentProductIndex];
    },
    filteredProducts() {
      return this.products.filter((product) => {
        return product.category === "men's clothing" || product.category === "women's clothing";
      });
    },
    currentFilteredProduct() {
      return this.filteredProducts[this.currentProductIndex];
    },
    ismenClothing() {
      const category = this.currentFilteredProduct.category;
      return category === "men's clothing";
    },
    iswomenClothing() {
      const category2 = this.currentFilteredProduct.category;
      return category2 === "women's clothing";
    },
    titleStyle() {
      if (this.iswomenClothing) {
        return { color: '#720060' };
      } else if (this.ismenClothing) {
        return { color: '#002772' };
      } else {
        return {};
      }
    },
    priceStyle() {
      if (this.iswomenClothing) {
        return { color: '#720060' };
      } else if (this.ismenClothing) {
        return { color: '#002772' };
      } else {
        return {};
      }
    },
    button1Style() {
      if (this.iswomenClothing) {
        return { backgroundColor: '#720060', color: '#ffffff' };
      } else if (this.ismenClothing) {
        return { backgroundColor: '#002772', color: '#ffffff' };
      } else {
        return {};
      }
    },
    button2Style() {
      if (this.iswomenClothing) {
        return { backgroundColor: '#ffffff', color: '#720060', border: 'solid #720060 2px' };
      } else if (this.ismenClothing) {
        return { backgroundColor: '#ffffff', color: '#002772', border: 'solid #002772px' };
      } else {
        return {};
      }
    },
  },
  mounted() {
    this.fetchProducts();
  },

  methods: {
    async fetchProducts() {
      try {
        const responses = await axios.get('https://fakestoreapi.com/products');
        this.products = responses.data;
      } catch (error) {
        console.error('Error fetching products:', error);
      }
    },
    nextProduct() {
      if (this.filteredProducts.length > 1) {
        this.currentProductIndex = (this.currentProductIndex + 1) % this.products.length;
      }

      //   if (this.currentProductIndex >= this.products.length - 1) {
      //     console.log('produk udh habis oi');
      //   }
    },
  },
};
</script>

<style>
.bungkus {
  width: 100%;
  height: 1000px;
}
.bg-men {
  width: 100%;
  height: 1000px;
  background-color: #002772; /* Background color for men's clothing */
}
.bg-women {
  width: 100%;
  height: 1000px;
  background-color: #720060; /* Background color for women's clothing */
}
.no-product-img {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 300px;
  background-color: lightgray;
}
button {
  width: 40%;
}
</style>

<template>
  <div id="app" class="small-container">
    <h1>Products</h1>
    <dropdown-list v-if="products.length > 0" :products="products" v-on:on-item-selected="handleChange($event)" @on-item-reset="dropdownSelection = ''" />
    {{ dropdownSelection }}
    <products-grid :isCategorySelected="isCategorySelected"  :products="products" v-bind:dropdownSelection="dropdownSelection" @on-item-selected="isCategorySelected = true"/>
  </div>
</template>

<script>
import ProductsGrid from "./components/Products";
import DropdownList from './components/DropdownList'

export default {
  name: 'app',
  components: {
    ProductsGrid,
    DropdownList
  },
  data() {
    return {
      products: [],
      dropdownSelection: "",
      isCategorySelected: false
    }
  },

  mounted() {
    this.getProducts()
  },
  methods: {
    async getProducts() {
      try {
        const response = await fetch('https://api.konimbo.co.il/v1/items?token=9c1a92bf8cefc59e4ec9fa7c53bba0f90dd8b15850bef1062dbf32c5e8fd3a08');
        const data = await response.json();
        this.products = data;
      } catch (error) {
        console.error(error)
      }
    },

    handleChange(event){
      this.dropdownSelection = event;
      this.isCategorySelected = true;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<template>
    <div id="products-grid">
        <table>
            <thead>
            <tr>
                <th>Product title</th>
                <th>Product image</th>
            </tr>
            </thead>
            <tbody v-if="!dropdownSelection">
            <tr  v-for="product in products" :key="product.id">
                <td>{{ product.title }}</td>
                <td v-if="product.images[0] !== undefined"><img :src="product.images[0].url"/></td>
                <td v-else >No image</td>
            </tr>
            </tbody>
            <tbody  v-else>
            <tr v-for="product in filteredProducts" :key="product.id">
                <td>{{ product.title }}</td>
                <td v-if="product.images[0] !== undefined"><img :src="product.images[0].url"/></td>
                <td v-else >No image</td>
            </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        name: 'products-grid',
        props: {
            products: Array,
            dropdownSelection: String,
            isCategorySelected: Boolean
        },
        data () {
            return {
                filteredProducts: [],
            }
        },
        updated() {
            if (this.isCategorySelected) {
                this.getProductsFiltered(this.dropdownSelection);
            }
        },
        methods: {
            getProductsFiltered(category){
                for (let i = 0; i < this.products.length ; i++){
                    if (this.products[i].store_category_title === category) {
                        this.filteredProducts.push(this.products[i])
                    }
                }
            }
        }
    }
</script>

<style scoped></style>
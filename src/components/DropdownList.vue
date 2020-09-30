
<template>
    <div class="dropdown">
        <input v-if="Object.keys(selectedItem).length === 0" ref="dropdowninput" v-model.trim="inputValue" class="dropdown-input" type="text" placeholder="Find category" />
        <div v-else @click="resetSelection" class="dropdown-selected">
            {{ selectedItem }}
        </div>
        <div v-show="inputValue && categoriesList.length > 0" class="dropdown-list">
            <div @click="selectItem(category)" v-show="category" v-for="category in categoriesList" :key="category" class="dropdown-item">
                {{ category }}
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'dropdown-list',
        props: {
            products: Array,
        },
        data () {
            return {
                selectedItem: {},
                inputValue: '',
                categoriesList: []
            }
        },
        mounted(){
            this.getCategories();
        },

        methods: {
            resetSelection () {
                this.selectedItem = {}
                this.$nextTick( () => this.$refs.dropdowninput.focus() )
                this.$emit('on-item-reset')
            },
            selectItem (theItem) {
                console.log("theItem : ", theItem);
                this.selectedItem = theItem
                this.inputValue = ''
                this.$emit('on-item-selected', theItem)
            },
            getCategories() {
                for (let i = 0; i < this.products.length; i++){
                    if (!this.categoriesList.includes(this.products[i].store_category_title)) {
                        this.categoriesList.push(this.products[i].store_category_title);
                    }
                }
            }
        }
    }
</script>

<style>
    .dropdown{
        position: relative;
        width: 100%;
        max-width: 400px;
        margin: 0 auto;
    }
    .dropdown-input, .dropdown-selected{
        width: 100%;
        padding: 10px 16px;
        border: 1px solid transparent;
        background: #edf2f7;
        line-height: 1.5em;
        outline: none;
        border-radius: 8px;
    }
    .dropdown-input:focus, .dropdown-selected:hover{
        background: #fff;
        border-color: #e2e8f0;
    }
    .dropdown-input::placeholder{
        opacity: 0.7;
    }
    .dropdown-selected{
        font-weight: bold;
        cursor: pointer;
    }
    .dropdown-list{
        position: absolute;
        width: 100%;
        max-height: 500px;
        margin-top: 4px;
        overflow-y: auto;
        background: #ffffff;
        box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        border-radius: 8px;
    }
    .dropdown-item{
        display: flex;
        width: 100%;
        padding: 11px 16px;
        cursor: pointer;
    }
    .dropdown-item:hover{
        background: #edf2f7;
    }
</style>
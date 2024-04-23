<template>
    <div class="columns is-multiline">
        <div class="column is-9">
            <figure class="image mb-6">
                <img v-bind:src="product.get_image" alt="">
            </figure>

            <h1 class="title"> {{product.name}}</h1>     
            
            <p>{{ product.description }}</p>
        </div>

        <div class="column is-3">
            <h2 class="subtitle">Information</h2>

            <p><strong>Price: </strong>RM{{product.price}}</p>

            <div class="field has-addons mt-6">
                <div class="control">
                    <input type="number" class="input" min="1" v-model="quantity">
                </div>

                <div class="control">
                    <a class="button is-dark" @click="addToCart">Add to cart</a>
                </div>
            </div>
        </div>

    </div>

</template>

<script>
import axios from 'axios'
import { toast }  from 'bulma-toast'

export default {
    name: 'Product',
    data() {
        return {
            product: {},
            quantity: 1
        }
    },
    mounted() {
        this.getProduct()
    },
    methods: {
        async getProduct() {
            this.$store.commit('setIsLoading', true)

            const category_slug = this.$route.params.category_slug
            const product_slug = this.$route.params.product_slug

            await axios
                .get(`/api/v1/products/${category_slug}/${product_slug}`)
                .then(response => {
                    this.product = response.data

                    document.title = this.product.name + ' | VueD'
                })
                .catch(error => {
                    console.log(error)
                })
            this.$store.commit('setIsLoading', false)

        },
        addToCart() {
            if (isNaN(this.quantity) || this.quantity < 1) {
                this.quantity = 1
            }

            const item = {
                product: this.product,
                quantity: this.quantity
            }
            this.$store.commit('addToCart', item)

            toast({
                message : "The product was added to the cart",
                type : "is-success",
                position : "bottom-right",
                dismissible : true,
                duration : 3000,
                pauseOnHover : true
            })
        }
    }
}
</script>


<style scoped>
/* Styling adjustments to improve the appearance */

/* Product details container */
.columns.is-multiline {
    justify-content: center; /* Center align the columns */
    margin-top: 2rem; /* Add some space at the top */
}

.column.is-9 {
    text-align: center; /* Center align the product details */
}

.title {
    font-size: 2.5rem; /* Increase title font size */
    margin-bottom: 1rem; /* Add some space below the title */
}

.subtitle {
    font-size: 1.5rem; /* Increase subtitle font size */
    margin-bottom: 1rem; /* Add some space below the subtitle */
}

.image {
    max-width: 100%; /* Ensure the image fits within its container */
    border-radius: 10px; /* Add border-radius for rounded corners */
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Add a subtle shadow for depth */
}

/* Product information container */
/* Product information container */
.column.is-3 {
    background-color: #292929; /* Dark background color */
    color: #fff; /* Light text color */
    padding: 2rem; /* Add padding to the product information container */
    border-radius: 10px; /* Add border-radius for rounded corners */
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Add a subtle shadow for depth */
}

.field {
    margin-top: 1.5rem; /* Add some space between fields */
}

.control {
    margin-bottom: 1rem; /* Add some space between controls */
}

.input {
    width: 80px; /* Set width of the input field */
}

.button.is-dark {
    width: 100%; /* Make the button full width */
}


</style>

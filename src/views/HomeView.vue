<template>
  <div class="home">
    <section class="hero is-medium is-dark mb-6">
        <div class="hero-body has-text-centered">
            <p class="title mb-6">
                Welcome to VueD
            </p>
            <p class="subtitle">
                The best jacket store online
            </p>
        </div>
    </section>

    <div class="columns is-multiline">
      <div class="column is-12">
          <h2 class="is-size-2 has-text-centered">Latest products</h2>
      </div>

      <ProductBox 
        v-for="product in latestProducts"
        v-bind:key="product.id"
        v-bind:product="product" />
    </div>
  </div>
</template>

<script>
import axios from 'axios'

import ProductBox from '@/components/ProductBox'

export default {
  name: 'Home',
  data() {
    return {
      latestProducts: []
    }
  },
  components: {
    ProductBox
  },
  mounted() {
    this.getLatestProducts()

    document.title = 'Home | VueD'
  },
  methods: {
    async getLatestProducts() {
      this.$store.commit('setIsLoading', true)

      await axios
        .get('/api/v1/latest-products/')
        .then(response => {
          this.latestProducts = response.data
        })
        .catch(error => {
          console.log(error)
        })

      this.$store.commit('setIsLoading', false)
    }
  }
}
</script>

<style scoped>
/* Styling adjustments to improve the appearance */

/* Hero section */
.hero.is-medium {
    background: linear-gradient(250deg, #48c774, #256c3b); /* Add a gradient background with shades of green */
    color: #fff; /* Light text color */
    padding: 3rem 0; /* Add padding to the hero section */
    border-radius: 10px;
}

.title {
    font-size: 3rem; /* Increase title font size */
    margin-bottom: 1rem; /* Add some space below the title */
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5); /* Add text shadow for better readability */
}

.subtitle {
    font-size: 1.5rem; /* Increase subtitle font size */
}

/* Latest products section */
.columns.is-multiline {
    justify-content: left; /* Center align the product columns */
}

.column.is-12 {
    text-align: center; /* Center align the Latest products heading */
}

.is-size-2 {
    margin-bottom: 2rem; /* Add more space below the Latest products heading */
}

</style>

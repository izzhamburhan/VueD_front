<template>
  <div class="wrapper">
    <nav class="navbar is-dark">
      <div class="navbar-brand">
        <router-link to="/" class="navbar-item"><strong>VueD</strong></router-link> 

        <a class="navbar-burger" aria-label="menu" aria-expanded="false" data-targets="navbar-menu" @click="showMobileMenu = !showMobileMenu">
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>

      <div class="navbar-menu" id="navbar-menu" v-bind:class="{'is-active':showMobileMenu }">
        <div class="navbar-end">
          <router-link to="/summer" class="navbar-item">Summer</router-link>
          <router-link to="/winter" class="navbar-item">Winter</router-link>

          <div class="navbar-item">
            <div class="buttons">
              <router-link to="/log-in" class="button is-light">Log In</router-link>

              <router-link to="/cart" class="button is-success">
                <span class="icon"><i class="fas fa-shopping-cart"></i></span>
                <span>Cart ({{ cartTotalLength }})</span>
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </nav>

    <div class="is-loading-bar has-text-centered" v-bind:class="{'is-loading': $store.state.isLoading}">
      <div class="lds-dual-ring"></div>
    </div>

    <section class="section">
      <router-view/>
    </section>

    <footer class="footer">
      <p class="has-text-centered"> &copy; 2024 NuxD. All rights reserved. </p>
    </footer>

  </div>

</template>

<script>
export default {
 data() {
  return {
    showMobileMenu : false,
    cart :{
      items : []
    }
  }
 },
 beforeCreate() {
  this.$store.commit('initializeStore');
 },
 mounted() {
  this.cart = this.$store.state.cart
 },
 computed : {
  cartTotalLength() {
   let totalLength = 0

   for  (let i=0;i<this.cart.items.length;i++) {
    totalLength += this.cart.items[i].quantity
   }
   return totalLength
  }
 }
}
</script>

<style lang="scss">
@import  '../node_modules/bulma';

// Customize navbar
.navbar {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.navbar-item {
  font-size: 1.1rem;
}

.navbar-burger {
  color: #fff;
}

// Customize footer
.footer {
  background-color: #363636;
  color: #fff;
  padding: 2rem 0;
}

// Additional custom styling
.wrapper {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.section {
  flex: 1;
}

.router-link-active {
  color: #48c774 !important;
}


/////// loading css /////////
.lds-dual-ring {
  display: inline-block;
  width: 80px;
  height: 80px;
}
.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 6px solid #ccc;
  border-color: #ccc transparent #ccc transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.is-loading-bar {
  height: 0;
  overflow: hidden;

  -webkit-transition: all 0.3s;
  transition: all 0.3s;

  &.is-loading {
    height: 80px;
  }
}
</style>

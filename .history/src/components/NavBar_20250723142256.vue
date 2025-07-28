<script setup>
import { BContainer, BNavbar, BNavbarBrand, BNavbarToggle } from 'bootstrap-vue-next'

import { ref, computed } from 'vue'
import { useCartStore } from '@/stores/CartStore'

const cartStore = useCartStore()
const cartCount = computed(() => cartStore.cartItems.length)

// Controls the visibility of the mobile full-page overlay menu
const showMobileMenu = ref(false)

// Toggle menu visibility
function toggleMenu() {
  showMobileMenu.value = !showMobileMenu.value
}
</script>

<template>
  <BNavbar toggleable="md" class="app-nav-bar py-3">
    <BContainer class="nav-container">
      <!-- Left: Toggle button + Brand -->
      <div class="d-flex justify-content-between align-items-center w-100">
        <!-- Hamburger toggle (mobile) -->
        <button class="hamburger-btn d-md-none" @click="toggleMenu">
          <i class="bi" :class="showMobileMenu ? 'bi-x-lg' : 'bi-list'"></i>
        </button>

        <!-- Brand -->
        <BNavbarBrand>
          <router-link to="/" class="brand-logo-link">
            <img src="@/assets/logo.png" alt="logo" width="30" />
            <span> Furniro</span>
          </router-link>
        </BNavbarBrand>

        <!-- Desktop icons -->
        <div class="nav-icons-md d-none d-md-flex align-items-center gap-3">
          <i class="bi bi-person-exclamation"></i>
          <i class="bi bi-heart"></i>
          <router-link to="/cart" class="cart-icon-wrapper">
            <i class="bi bi-cart"></i>
            <span class="cart-count-badge" v-if="cartCount > 0" :key="cartCount">{{
              cartCount
            }}</span>
          </router-link>
        </div>
      </div>
    </BContainer>
  </BNavbar>

  <!-- MOBILE FULLSCREEN MENU OVERLAY -->
  <div class="mobile-overlay-menu" v-if="showMobileMenu">
    <div class="overlay-content">
      <nav class="overlay-nav">
        <router-link to="/" class="overlay-link" @click="toggleMenu">Home</router-link>
        <router-link to="/shop" class="overlay-link" @click="toggleMenu">Shop</router-link>
        <router-link to="/about" class="overlay-link" @click="toggleMenu">About</router-link>
        <router-link to="/contact" class="overlay-link" @click="toggleMenu">Contact</router-link>
        <router-link to="/cart" class="overlay-link" @click="toggleMenu">
          Cart <span v-if="cartCount > 0">({{ cartCount }})</span>
        </router-link>
      </nav>
    </div>
  </div>
</template>

<style scoped>
/* Branding */
.brand-logo-link {
  display: flex;
  align-items: center;
  text-decoration: none;
  color: black;
  font-weight: bold;
  gap: 6px;
}

/* Hamburger button (mobile) */
.hamburger-btn {
  background: none;
  border: none;
  font-size: 24px;
  color: black;
}

/* Desktop nav icons */
.nav-icons-md i {
  font-size: 22px;
  color: black;
}

/* Cart badge styling */
.cart-icon-wrapper {
  position: relative;
  display: inline-block;
}
.cart-count-badge {
  position: absolute;
  top: -2px;
  right: -8px;
  background-color: rgba(184, 142, 47, 1);
  color: white;
  border-radius: 50%;
  font-size: 14px;
  width: 16px;
  height: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  animation: pop 0.3s ease;
}
@keyframes pop {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}

/* Overlay menu styling (mobile only) */
.mobile-overlay-menu {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 9999;
  width: 100vw;
  height: 100vh;
  background: white;
  display: flex;
  align-items: center;
  justify-content: center;
}
.overlay-content {
  text-align: center;
}
.overlay-nav {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
.overlay-link {
  font-size: 22px;
  color: black;
  text-decoration: none;
  font-weight: bold;
}
.overlay-link:hover {
  color: rgba(220, 170, 60, 1);
}
</style>

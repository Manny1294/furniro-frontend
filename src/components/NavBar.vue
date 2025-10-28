<script setup>
import {
  BContainer,
  BNavbar,
  BNavbarBrand,
  BCollapse,
  BNavbarNav,
  BNavItem,
} from 'bootstrap-vue-next'

import { computed, ref } from 'vue'
import { useCartStore } from '@/stores/CartStore'

// access the store to get cart data(total items count)
const cartStore = useCartStore()

//computed property to get the no. of items in the cart
const cartCount = computed(() => cartStore.cartItems.length)

//state to track if the menu is open or closed
const isMenuOpen = ref(false)

//function to open the menu overlay
const openMenu = () => {
  isMenuOpen.value = true
}

//function to close the menu overlay
const closeMenu = () => {
  isMenuOpen.value = false
}
</script>

<template>
  <BNavbar toggleable="md " class="app-nav-bar py-3">
    <BContainer class="nav-container">
      <!-- Top Section: Hamburger, Brand Logo/Name , Icons (all inline) -->
      <div class="d-flex">
        <!-- Left side: Hamburger + Brand -->
        <div class="toggle-brand">
          <!-- Hamburger(only on mobile) -->
          <button class="hamburger-btn d-md-none" @click="openMenu">
            <i class="bi bi-list"></i>
          </button>

          <!--  Logo/Brand -->
          <BNavbarBrand>
            <router-link to="/" class="brand-logo-link">
              <img src="@/assets/logo.png" alt="logo" width="30" />
              <span> Furniro</span>
            </router-link>
          </BNavbarBrand>
        </div>

        <!-- Right Side:  Icons (mobile  only) -->
        <div class="nav-icons">
          <i class="bi bi-person-exclamation"></i>
          <i class="bi bi-heart"></i>
          <!-- cart icon with  link to cart summary page -->
          <router-link to="/cart" class="cart-icon-wrapper">
            <i class="bi bi-cart"></i>
            <!-- only show if greater than 0 -->
            <span class="cart-count-badge" v-if="cartCount > 0">{{ cartCount }}</span>
          </router-link>
        </div>
      </div>

      <!-- Desktop nav-links -->
      <BCollapse id="nav-collapse" is-nav class="mobile-overlay-collapse">
        <BNavbarNav class="justify-content-center w-100">
          <BNavItem>
            <router-link to="/" class="nav-link">Home</router-link>
          </BNavItem>
          <BNavItem>
            <router-link to="/shop" class="nav-link">Shop</router-link>
          </BNavItem>
          <BNavItem>
            <router-link to="/about" class="nav-link">About</router-link>
          </BNavItem>
          <BNavItem>
            <router-link to="/contact" class="nav-link">Contact</router-link>
          </BNavItem>
        </BNavbarNav>
      </BCollapse>

      <!-- icons for desktop-->
      <div class="nav-icons-md">
        <i class="bi bi-person-exclamation"></i>
        <i class="bi bi-search"></i>
        <i class="bi bi-heart"></i>
        <router-link to="/cart" class="cart-icon-wrapper">
          <i class="bi bi-cart"></i>

          <!-- only show if greater than 0, :key retriggers the animation by re-rendering the badge each time the number changes -->
          <span class="cart-count-badge" v-if="cartCount > 0" :key="cartCount">{{
            cartCount
          }}</span>
        </router-link>
      </div>
    </BContainer>
  </BNavbar>

  <!-- Full screen overlay (Mobile Menu) -->
  <transition name="slide-overlay">
    <div v-if="isMenuOpen" class="mobile-overlay">
      <!-- Close Button  -->
      <button class="close-btn" @click="closeMenu">
        <i class="bi bi-arrow-left-short"></i>
      </button>

      <!-- Overlay Navigation Links (Mobile)-->
      <nav class="overlay-nav-links">
        <router-link to="/" class="overlay-link" @click="closeMenu">Home</router-link>
        <router-link to="/shop" class="overlay-link" @click="closeMenu">Shop</router-link>
        <router-link to="/about" class="overlay-link" @click="closeMenu">About</router-link>
        <router-link to="/contact" class="overlay-link" @click="closeMenu">Contact</router-link>
      </nav>
    </div>
  </transition>
</template>

<style scoped>
.toggle-brand {
  display: flex;
  gap: 10px;
  width: 100%;
  align-items: center;
}
.brand-logo-link {
  display: flex;
  align-items: center;
  text-decoration: none;
  color: black;
  font-weight: bold;
  gap: 6px;
}

.nav-icons {
  display: flex;
  align-items: center;
  gap: 20px;
}

.nav-icons-md {
  display: none;
}
.nav-icons i {
  font-size: 22px;
  color: black;
}

.nav-link {
  font-size: 16px;
  color: black;
  font-weight: 500;
}
.nav-link:hover {
  color: rgba(220, 170, 60, 1);
}
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
  transition: transform 0.3s;
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

/* HAMBURGER BUTTON */
.hamburger-btn {
  background: none;
  border: none;
  font-size: 30px;
  color: black;
  cursor: pointer;
}
/* OVERLAY MENU */
.mobile-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: white;
  z-index: 9999;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
/* CLOSE  BUTTON */
.close-btn {
  position: absolute;
  top: 20px;
  right: 20px;
  background: none;
  border: none;
  font-size: 24px;
  color: black;
  cursor: pointer;
}

/* LINKS IN OVERLAY */
.overlay-nav-links {
  display: flex;
  flex-direction: column;
  gap: 20px;
  text-align: center;
}

.overlay-link {
  text-decoration: none;
  font-size: 20px;
  color: black;
  font-weight: bold;
}
.overlay-link:hover {
  color: rgba(220, 170, 60, 1);
}
/* Slide-in transition for overlay */
.slide-overlay-enter-active,
.slide-overlay-leave-active {
  transition: all 0.3s ease;
}

.slide-overlay-enter-from {
  transform: translateX(-100%);
  opacity: 0;
}

.slide-overlay-enter-to {
  transform: translateX(0);
  opacity: 1;
}

.slide-overlay-leave-from {
  transform: translateX(0);
  opacity: 1;
}

.slide-overlay-leave-to {
  transform: translateX(-100%);
  opacity: 0;
}

/* Animate each overlay link */
.overlay-nav-links {
  display: flex;
  flex-direction: column;
  gap: 20px;
  margin-top: 60px;
  padding: 0 20px;
}

.overlay-link {
  opacity: 0;
  transform: translateY(20px);
  animation: slideFadeIn 0.4s ease forwards;
}

/* Individual delays for staggered effect */
.overlay-link:nth-child(1) {
  animation-delay: 0.3s;
}
.overlay-link:nth-child(2) {
  animation-delay: 0.4s;
}
.overlay-link:nth-child(3) {
  animation-delay: 0.5s;
}
.overlay-link:nth-child(4) {
  animation-delay: 0.6s;
}

/* Keyframes */
@keyframes slideFadeIn {
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

/* larger screen size */
@media (min-width: 768px) {
  .nav-container {
    display: flex;
    /* align-items: center; */
    padding: 0%;
  }
  .nav-icons {
    display: none;
  }
  .nav-icons-md {
    display: flex;
    gap: 20px;
    align-items: center;
  }

  nav-icons-md i {
    font-size: 22px;
    color: black;
  }

  .cart-icon-wrapper .bi-cart {
    color: rgb(0, 0, 0);
    transition: color 0.3s ease;
  }

  .cart-icon-wrapper:hover .bi-cart {
    color: rgba(220, 170, 60, 1);
  }
}
</style>

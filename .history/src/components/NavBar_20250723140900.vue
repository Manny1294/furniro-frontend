<script setup>
import { BContainer, BNavbar, BNavbarBrand } from 'bootstrap-vue-next'

import { computed, ref } from 'vue'
import { useCartStore } from '@/stores/CartStore'

// Get cart store instance
const cartStore = useCartStore()

// Computed property for total cart items
const cartCount = computed(() => cartStore.cartItems.length)

// Function to handle cart icon click (currently logs a message)
function handleCartClick() {
  console.log('🛒 Cart icon clicked!')
}

// State to toggle mobile menu panel
const isMobileMenuOpen = ref(false)

// Toggles mobile menu on or off
function toggleMobileMenu() {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}
</script>

<template>
  <BNavbar toggleable="md" class="app-nav-bar py-3">
    <BContainer class="nav-container">
      <!-- Top row: toggle + brand + icons -->
      <div class="d-flex w-100 justify-content-between align-items-center">
        <!-- Toggle button and brand -->
        <div class="toggle-brand">
          <!-- Replacing <BNavbarToggle> with custom button -->
          <button @click="toggleMobileMenu" class="menu-toggle-btn">
            <i class="bi bi-list"></i>
          </button>

          <!-- Brand logo and name -->
          <BNavbarBrand>
            <router-link to="/" class="brand-logo-link">
              <img src="@/assets/logo.png" alt="logo" width="30" />
              <span>Furniro</span>
            </router-link>
          </BNavbarBrand>
        </div>

        <!-- Right side icons (mobile view) -->
        <div class="nav-icons">
          <i class="bi bi-person-exclamation"></i>

          <!-- Removed search icon -->

          <router-link to="/cart" class="cart-icon-wrapper" @click="handleCartClick">
            <i class="bi bi-cart"></i>
            <!-- Cart count badge (only shows if greater than 0) -->
            <span class="cart-count-badge" v-if="cartCount > 0">{{ cartCount }}</span>
          </router-link>
        </div>
      </div>

      <!-- Custom mobile nav overlay (replaces BCollapse) -->
      <div v-if="isMobileMenuOpen" class="mobile-menu-overlay">
        <ul class="mobile-menu-links">
          <li><router-link to="/" @click="toggleMobileMenu">Home</router-link></li>
          <li><router-link to="/shop" @click="toggleMobileMenu">Shop</router-link></li>
          <li><router-link to="/about" @click="toggleMobileMenu">About</router-link></li>
          <li><router-link to="/contact" @click="toggleMobileMenu">Contact</router-link></li>
        </ul>
      </div>

      <!-- Desktop icons -->
      <div class="nav-icons-md">
        <i class="bi bi-person-exclamation"></i>

        <!-- Removed search icon -->

        <i class="bi bi-heart"></i>

        <router-link to="/cart" class="cart-icon-wrapper" @click="handleCartClick">
          <i class="bi bi-cart"></i>
          <span class="cart-count-badge" v-if="cartCount > 0" :key="cartCount">
            {{ cartCount }}
          </span>
        </router-link>
      </div>
    </BContainer>
  </BNavbar>
</template>

<style scoped>
.toggle-brand {
  display: flex;
  gap: 10px;
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

/* Custom mobile menu full-screen overlay */
.mobile-menu-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background: white;
  z-index: 1000;
  padding: 2rem 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  font-size: 20px;
}

.mobile-menu-links {
  list-style: none;
  padding: 0;
  margin: 0;
}

.mobile-menu-links li {
  margin-bottom: 1rem;
}

.menu-toggle-btn {
  background: none;
  border: none;
  font-size: 26px;
}

/* Desktop styles */
@media (min-width: 768px) {
  .nav-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .nav-icons {
    display: none;
  }

  .nav-icons-md {
    display: flex;
    gap: 20px;
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

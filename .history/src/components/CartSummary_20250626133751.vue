<script setup>
import { useCartStore } from '@/stores/CartStore'
import { BButton } from 'bootstrap-vue-next'
import { useRouter } from 'vue-router'
import { BContainer } from 'bootstrap-vue-next'

const cart = useCartStore()
const router = useRouter()

// remove item from cart
function removeItem(id) {
  cart.removeFromCart(id)
}

// function to route to product description
function goToProduct(id) {
  router.push(`/product/${id}`)
}

function checkout() {
  router.push('/order-success')
}
</script>

<template>
  <BContainer>
    <div class="cart__wrapper">
      <!-- Scrollable Table -->
      <div class="cart__table-scroll">
        <table class="cart__tabletable table-borderless">
          <thead>
            <tr>
              <th>Product</th>
              <th>Price</th>
              <th>Quantity</th>
              <th>Subtotal</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in cart.cartItems" :key="item.id">
              <td @click="goToProduct(item.id)" style="cursor: pointer">
                <div class="product-info">
                  <img :src="item.image" alt="product" />
                  <span>{{ item.title }}</span>
                </div>
              </td>
              <td>₦ {{ item.price.toLocaleString() }}</td>
              <td>
                <input
                  type="number"
                  :id="`qty-${item.id}`"
                  v-model.number="item.quantity"
                  min="1"
                  max="10"
                  class="qty-text"
                  @input="
                    () => {
                      if (item.quantity > 10) item.quantity = 10
                      else if (item.quantity < 1) item.quantity = 1
                    }
                  "
                />
              </td>
              <td>₦ {{ (item.price * item.quantity).toLocaleString() }}</td>

              <td>
                <i
                  class="bi bi-trash3-fill"
                  @click="removeItem(item.id)"
                  style="color: #b88e2f; font-size: 1.3rem; cursor: pointer"
                ></i>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <!-- Cart Totals -->
      <div class="total-card">
        <div class="total-content">
          <h3>Cart Totals</h3>
          <div class="content-one">
            <p>Subtotal</p>
            <p>₦ {{ cart.subtotal.toLocaleString() }}</p>
          </div>
          <div class="content-one">
            <p>Total</p>
            <p class="total-price">₦ {{ cart.subtotal.toLocaleString() }}</p>
          </div>
        </div>
        <BButton class="checkout-btn" @click="checkout">Check Out</BButton>
      </div>
    </div>
  </BContainer>
</template>

<style scoped>
.cart__wrapper {
  display: flex;
  flex-direction: column;
  gap: 24px;
  padding: 16px;
}

.cart__table-scroll {
  overflow-x: auto;
  width: 100%;
}

.cart__table {
  min-width: 600px;
  background: #f9f1e7;
}

.cart__tableth,
.cart__tabletd {
  vertical-align: middle;
  max-width: 200px; /* Prevents overly wide cells */
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.product-info {
  display: flex;
  align-items: center;
  gap: 10px;
  justify-content: flex-start;
  max-width: 250px;
}

.product-info img {
  width: 40px;
  height: 40px;
  object-fit: contain;
}

.qty-text {
  width: 50px;
  text-align: center;
  border: 1px solid #9f9f9f;
  border-radius: 5px;
}

.total-card {
  background-color: #f9f1e7;
  padding: 24px;
  height: fit-content;
}

.total-content {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.content-one {
  display: flex;
  justify-content: space-between;
}

.total-price {
  color: #b88e2f;
  font-size: 20px;
  font-weight: 400;
}

.checkout-btn {
  background-color: rgba(184, 142, 47, 1);
  color: #ffffff;
  width: 100%;
  border: none;
  border-radius: 0;
  text-align: center;
  padding: 12px 0;
  font-size: 16px;
  font-weight: 400;
  margin-top: 20px;
  transition:
    background-color 0.3s ease,
    color 0.3s ease,
    transform 0.2s ease;
}
.checkout-btn:hover {
  background-color: rgba(184, 142, 47, 1); /* gold */
  color: #000;
  border-color: rgba(184, 142, 47, 1);
  transform: scale(1.03);
}
.checkout-btn:active {
  background-color: rgba(184, 142, 47, 0.8);
  transform: scale(0.97);
}

.cart__tablethead {
  color: blue;
}

/* Desktop side-by-side layout */
@media (min-width: 1024px) {
  .cart__wrapper {
    flex-direction: row;
    align-items: flex-start;
    justify-content: space-between;
    gap: unset;
  }

  .cart__table-scroll {
    overflow-x: unset;
  }

  .total-card {
    background-color: #f9f1e7;
    padding: 24px;
    height: fit-content;
    max-width: 350px;
    width: 100%;
  }

  .product-info {
    max-width: 250px;
  }
}
</style>

<template>
  <div>
    <Navbar :cartCount="cartCount" />

    <div class="container mt-4">
      <div class="d-flex justify-content-between align-items-center mb-3">
        <div>
          <button class="btn btn-outline-secondary me-2" @click="showProducts = !showProducts">
            {{ showProducts ? "Hide Products" : "Show Products" }}
          </button>
          <button class="btn btn-outline-danger" @click="clearCart" :disabled="cart.length === 0">
            Clear Cart
          </button>
        </div>

        <div class="text-end">
          <p v-if="discount > 0" class="mb-0">
            🎉 You have a <strong>{{ discount }}%</strong> discount on checkout!
          </p>
          <small class="text-muted">Products: {{ products.length }}</small>
        </div>
      </div>

      <p v-if="products.length === 0" class="text-center text-muted">No products available</p>

      <div v-show="showProducts" class="row g-3">
        <div v-for="product in products" :key="product.id" class="col-12 col-md-6 col-lg-4">
          <ProductCard :product="product" @add-to-cart="addToCart" />
        </div>
      </div>

      <hr class="my-4" />

      <div class="row">
        <div class="col-md-6">
          <Cart :cart="cart" @remove-from-cart="removeFromCart" />
        </div>

        <div class="col-md-6">
          <div class="card p-3">
            <h5>Order Summary</h5>
            <p class="mb-1">Items in cart: <strong>{{ cart.length }}</strong></p>
            <p class="mb-1">Subtotal: <strong>\${{ totalPrice.toFixed(2) }}</strong></p>
            <p v-if="discount > 0" class="mb-1 text-success">
              Discount ({{ discount }}%): <strong>-\${{ (totalPrice * (discount/100)).toFixed(2) }}</strong>
            </p>
            <hr />
            <p class="mb-0">Total: <strong>\${{ finalPrice.toFixed(2) }}</strong></p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "./components/Navbar.vue";
import ProductCard from "./components/ProductCard.vue";
import Cart from "./components/Cart.vue";

export default {
  name: "App",
  components: { Navbar, ProductCard, Cart },
  data() {
    return {
      products: [
        {
          id: 1,
          name: "iPhone 15",
          price: 1199,
          inStock: true,
          description: "Latest Apple iPhone with A17 chip.",
          image: "https://picsum.photos/400/250?random=1"
        },
        {
          id: 2,
          name: "Samsung Galaxy S23",
          price: 999,
          inStock: false,
          description: "Flagship Samsung phone with powerful camera.",
          image: "https://picsum.photos/400/250?random=2"
        },
        {
          id: 3,
          name: "Sony WH-1000XM5",
          price: 349,
          inStock: "limited", // لو عايز تعرض limited stock خليها كده
          description: "Industry-leading noise cancelling headphones.",
          image: "https://picsum.photos/400/250?random=3"
        },
        {
          id: 4,
          name: "MacBook Pro 14",
          price: 1999,
          inStock: true,
          description: "M2 Pro chip, Liquid Retina XDR display.",
          image: "https://picsum.photos/400/250?random=4"
        },
        {
          id: 5,
          name: "Google Pixel 8",
          price: 799,
          inStock: true,
          description: "Pure Android experience with stellar camera.",
          image: "https://picsum.photos/400/250?random=5"
        },
        {
          id: 6,
          name: "Bose SoundLink",
          price: 129,
          inStock: false,
          description: "Portable Bluetooth speaker.",
          image: "https://picsum.photos/400/250?random=6"
        }
      ],
      cart: [],
      cartCount: 0,
      showProducts: true,
      discount: 10
    };
  },
  computed: {
    totalPrice() {
      return this.cart.reduce((sum, p) => sum + Number(p.price), 0);
    },
    finalPrice() {
      if (this.discount > 0) {
        return this.totalPrice * (1 - this.discount / 100);
      }
      return this.totalPrice;
    }
  },
  methods: {
    addToCart(product) {
      if (!product.inStock || product.inStock === false) {
        alert(`${product.name} is out of stock and cannot be added.`);
        return;
      }
      this.cart.push({ ...product });
      this.cartCount = this.cart.length;
      alert(`${product.name} added to cart!`);
    },
    removeFromCart(index) {
      if (index >= 0 && index < this.cart.length) {
        this.cart.splice(index, 1);
        this.cartCount = this.cart.length;
      }
    },
    clearCart() {
      if (this.cart.length === 0) return;
      if (confirm("Are you sure you want to clear the cart?")) {
        this.cart = [];
        this.cartCount = 0;
      }
    }
  }
};
</script>

<style>
.out-of-stock {
  opacity: 0.6;
  filter: grayscale(60%);
}
.in-stock {
  border: 2px solid rgba(16, 185, 129, 0.12);
}
.limited-stock {
  border: 2px solid rgba(249, 115, 22, 0.12);
}
</style>

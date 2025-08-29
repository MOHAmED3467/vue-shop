<template>
  <nav class="navbar navbar-dark bg-dark">
    <div class="container">
      <span class="navbar-brand mb-0 h1">Vue Shop Pro</span>
      <div>
        <button class="btn btn-sm btn-outline-light me-2" @click="$emit('toggle-cart')">
          <!-- optional: can be used to show cart modal -->
          View Cart
        </button>
        <span class="badge bg-primary">Cart: {{ cartCount }}</span>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Navbar",
  props: {
    cartCount: {
      type: Number,
      default: 0
    }
  }
};
</script>

<style scoped>
.navbar {
  padding: 0.6rem 0;
}
</style>

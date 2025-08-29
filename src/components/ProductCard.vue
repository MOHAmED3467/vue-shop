<template>
  <div :class="['card h-100', stockClass]" >
    <img :src="product.image" class="card-img-top" :alt="product.name" />
    <div class="card-body d-flex flex-column">
      <h5 class="card-title">{{ product.name }}</h5>
      <p class="card-text mb-1"><strong>\${{ product.price }}</strong></p>

      <p class="mb-1">
        <span v-if="product.inStock === true" class="in-stock-text">In Stock</span>
        <span v-else-if="product.inStock === 'limited'" class="limited-stock-text">Limited Stock</span>
        <span v-else class="out-of-stock-text">Out of Stock</span>
      </p>

      <p class="card-text small text-muted" v-text="product.description"></p>

      <div class="mt-auto d-flex justify-content-between align-items-center">
        <button
          class="btn btn-primary"
          :disabled="!product.inStock"
          @click="$emit('add-to-cart', product)"
        >
          Add to Cart
        </button>

        <small :class="statusColorClass" class="fw-bold text-end">
          <!-- visual stock label -->
          <span v-if="product.inStock === true">Available</span>
          <span v-else-if="product.inStock === 'limited'">Limited</span>
          <span v-else>Sold Out</span>
        </small>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductCard",
  props: {
    product: {
      type: Object,
      required: true
    }
  },
  computed: {
    stockClass() {
      if (this.product.inStock === true) return "in-stock";
      if (this.product.inStock === "limited") return "limited-stock";
      return "out-of-stock";
    },
    statusColorClass() {
      if (this.product.inStock === true) return "text-success";
      if (this.product.inStock === "limited") return "text-warning";
      return "text-muted";
    }
  }
};
</script>

<style scoped>
.card-img-top {
  object-fit: cover;
  height: 180px;
}
.out-of-stock {
  opacity: 0.6;
  filter: grayscale(60%);
}
.in-stock {
  box-shadow: 0 0 0 4px rgba(34,197,94,0.03);
}
.limited-stock {
  box-shadow: 0 0 0 4px rgba(249,115,22,0.03);
}
.in-stock-text { color: #16a34a; }
.limited-stock-text { color: #f59e0b; }
.out-of-stock-text { color: #6b7280; }
</style>

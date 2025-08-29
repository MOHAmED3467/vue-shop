<template>
  <div>
    <h5>Your Cart</h5>
    <ul class="list-group mb-2">
      <li
        v-for="(item, index) in cart"
        :key="index"
        class="list-group-item d-flex justify-content-between align-items-center"
      >
        <div class="d-flex align-items-center">
          <img :src="item.image" alt="" style="width:64px; height:48px; object-fit:cover; margin-right:12px;" />
          <div>
            <div class="fw-bold">{{ item.name }}</div>
            <small class="text-muted">\${{ item.price }}</small>
          </div>
        </div>

        <div>
          <button class="btn btn-sm btn-outline-danger" @click="$emit('remove-from-cart', index)">Remove</button>
        </div>
      </li>
    </ul>

    <p v-if="cart.length === 0" class="text-muted">Cart is empty</p>
  </div>
</template>

<script>
export default {
  name: "Cart",
  props: {
    cart: {
      type: Array,
      default: () => []
    }
  }
};
</script>

<style scoped>
.list-group-item img {
  border-radius: 6px;
}
</style>

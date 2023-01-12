<template>
  <div class="container py-3">
    <div class="row">
      <div class="col-12">
        <h4>Dashboard</h4>
        <p>
          Lorem ipsum dolor sit amet, consectetur adipisicing elit. A aspernatur blanditiis deserunt ducimus eaque facilis fuga ipsam nobis non odit officiis placeat rerum saepe tempore veritatis vero voluptatem, voluptates. Quam!
        </p>
        <router-link to="/products" class="btn btn-primary me-2"><i class="bi bi-boxes"></i> Product List</router-link>
        <router-link to="/products/create" class="btn btn-outline-primary"><i class="bi bi-plus-lg"></i> Add Product</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DashboardView",
}
</script>

<style scoped>

</style>
<template>
  <div class="container py-3">
    <div class="row">
      <div class="col-12">
        <h4 class="mb-4">Products List</h4>
        <div class="">
          <div class="w-25 float-end mb-3">
            <Search @search="search" />
          </div>
          <table class="table table-responsive table-hover">
            <thead>
            <tr>
              <th>#</th>
              <th>Product Name</th>
              <th>Price</th>
              <th>Stock</th>
              <th>Created</th>
              <th>Control</th>
            </tr>
            </thead>
            <tbody>
            <tr v-if="isLoading">
              <td colspan="6">
                <div class="d-flex justify-content-center">
                  <div class="spinner-border" role="status">
                    <span class="visually-hidden">Loading...</span>
                  </div>
                </div>
              </td>
            </tr>
            <tr v-else v-for="row in rows.data" :key="row.id">
              <td>{{ row.id }}</td>
              <td>{{ row.name }}</td>
              <td>{{ row.price }}</td>
              <td>{{ row.stock }}</td>
              <td>{{ row.date }}</td>
              <td>
                <div class="btn-group btn-group-sm">
                  <button @click="fetchProduct(row.id)" class="btn btn-outline-primary">
                    <i class="bi bi-info-square text-info small"></i>
                  </button>
                  <router-link :to="'/products/edit/'+row.id" class="btn btn-outline-primary">
                    <i class="bi bi-pencil small"></i>
                  </router-link>
                  <button @click="deleteProduct(row.id)" class="btn btn-outline-primary">
                    <i class="bi bi-trash text-danger small"></i>
                  </button>
                </div>
              </td>
            </tr>
            </tbody>
          </table>

          <!--vue pagination-->
          <div class="float-end">
            <Pagination @fetch-link="fetchProducts" v-if="rows.meta" :links="rows.meta.links" />
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import {mapGetters} from "vuex";
import Pagination from "@/components/Pagination";
import Search from "@/components/Search";

export default {
  name: "ProductsView",
  components: {Search, Pagination},
  data() {
    return {
      rows: {},
      isLoading: false
    }
  },
  computed: {
    ...mapGetters(['getUrl'])
  },
  methods: {
    search(keyword){
      //console.log(keyword)
      this.fetchProducts(this.getUrl('/products?keyword='+keyword))
    },
    showToast(icon,message){
      const Toast = this.$swal.mixin({
        toast: true,
        position: 'top-end',
        showConfirmButton: false,
        timer: 3000,
        timerProgressBar: true,
        didOpen: (toast) => {
          toast.addEventListener('mouseenter', this.$swal.stopTimer)
          toast.addEventListener('mouseleave', this.$swal.resumeTimer)
        }
      })

      Toast.fire({
        icon: icon,
        title:  message
      })
    },
    fetchProducts(url) {
      this.isLoading = true
      axios.get(url)
      .then(res => {
        // console.log(res.data)
        this.rows = res.data
      }).finally(_=> this.isLoading=false)
    },
    fetchProduct(id){
      axios.get(this.getUrl('/products/'+id)).then(res => console.log(res.data))
    },
    deleteProduct(id){
      axios.delete(this.getUrl('/products/'+id))
          .then(res => {
            this.fetchProducts(this.rows.meta.links.find(link => link.active === true).url)
            this.showToast('success',res.data.message)
          })
    }
  },
  mounted() {
    this.fetchProducts(this.getUrl('/products'))
  }
}
</script>

<style scoped>

</style>
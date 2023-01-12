<template>
  <div class="container py-3">
    <div class="row">
      <div class="col-12">
        <h4 class="mb-4">Edit Product</h4>
        <form action="" ref="productUpdate" @submit.prevent="productUpdate">
          <div class="row">
            <div class="col">
              <Input label="Product Name" name="name" :errors="errors" :value="product.name" />
            </div>
            <div class="col">
              <Input label="Price" type="number" name="price" :errors="errors" :value="product.price" />
            </div>
            <div class="col">
              <Input label="Stock" type="number" name="stock" :errors="errors" :value="product.stock" />
            </div>
            <div class="col">
              <button :disabled="isLoading" class="btn btn-primary btn-lg">
                <span v-if="isLoading" class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
                <i v-else class="bi bi-save"></i>
                Update Product
              </button>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import Input from "@/components/Input";
import {mapGetters} from "vuex";
import axios from "axios";
export default {
  name: "ProductEditView",
  components: {Input},
  data() {
    return {
      isLoading: false,
      errors: {},
      product: {}
    }
  },
  computed: {
    ...mapGetters([
      "getUrl"
    ])
  },
  methods: {
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
    productUpdate() {
      this.isLoading = true;
      let formData = new FormData(this.$refs.productUpdate)
      axios.put(this.getUrl('/products/'+this.$route.params.id),new URLSearchParams(formData).toString())
          .then(res => {
            this.errors = {}
            if (res.data.success === true){
              this.showToast('success',res.data.message)
              this.product = res.data.product
              this.$router.push('/products')
            }
          }).catch(e => {
        if (e.response.status === 422){
          this.errors = e.response.data.errors
          this.showToast('error',e.response.data.message)
        }
      }).finally(_=>this.isLoading = false)
    },
    fetchProduct(id){
      axios.get(this.getUrl('/products/'+id))
          .then(res => {
            this.product = res.data.data
          })
    }
  },
  mounted() {
    this.fetchProduct(this.$route.params.id)
  }
}
</script>

<style scoped>

</style>
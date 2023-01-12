<template>
  <div class="container py-3">
    <div class="row">
      <div class="col-12">
        <h4 class="mb-4">Create Product</h4>
        <form action="" ref="productCreate" @submit.prevent="productCreate">
          <div class="row">
            <div class="col-4">
              <Input label="Product Name" name="name" :errors="errors" />
            </div>
            <div class="col-4">
              <Input label="Price" type="number" name="price" :errors="errors" />
            </div>
            <div class="col-4">
              <Input label="Stock" type="number" name="stock" :errors="errors" />
            </div>
            <div class="col-12">
              <div class="mb-4">
                <label for="photos" class="form-label">Choose Photo</label>
                <input class="form-control" name="photos[]" :class="{'is-invalid':errors.photos}" type="file" id="photos" multiple>
                <div class="invalid-feedback" v-if="errors.photos">{{ errors.photos[0] }}</div>
              </div>
            </div>
            <div class="col-12">
              <button :disabled="isLoading" class="btn btn-primary">
                <span v-if="isLoading" class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
                <i v-else class="bi bi-save"></i>
                Add Product
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
import {debounce, throttle} from "lodash";
export default {
  name: "ProductCreateView",
  components: {Input},
  data() {
    return {
      isLoading: false,
      errors: {}
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
    productCreate:debounce(function () {
      this.isLoading = true
      let formData = new FormData(this.$refs.productCreate)
      axios.post(this.getUrl('/products'),formData)
          .then(res => {
            this.errors = {}
            if (res.data.success === true){
              this.showToast('success',res.data.message)
              this.$refs.productCreate.reset()
            }
          }).catch(e => {
        if (e.response.status === 422){
          this.errors = e.response.data.errors
          this.showToast('error',e.response.data.message)
        }
      }).finally(_=>this.isLoading = false)
    },500)
  },
}
</script>

<style scoped>

</style>
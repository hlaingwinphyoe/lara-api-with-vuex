<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-lg-4">
        <h1 class="my-4 text-center">Register Form</h1>
        <form action="" @submit.prevent="register" ref="registerForm">
          <div class="form-floating mb-3">
            <input type="text" class="form-control" id="name" name="name" placeholder="Name">
            <label for="name">Your Name</label>
          </div>
          <div class="form-floating mb-3">
            <input type="email" class="form-control" id="email" name="email" placeholder="name@example.com">
            <label for="email">Email address</label>
          </div>
          <div class="form-floating mb-3">
            <input type="password" class="form-control" id="password" name="password" placeholder="Password">
            <label for="password">Password</label>
          </div>
          <div class="form-floating mb-3">
            <input type="password" class="form-control" id="password_confirmation" name="password_confirmation" placeholder="Password">
            <label for="password_confirmation">Confirm Password</label>
          </div>
          <div class="mb-3">
            <button class="btn btn-primary"><i class="bi bi-box-arrow-in-right"></i> Register</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import {mapGetters} from "vuex";

export default {
  computed: {
    ...mapGetters([
        'getUrl'
    ])
  },
  methods: {
    register() {
      let formData = new FormData(this.$refs.registerForm);
      fetch(this.getUrl('/register'),{
        method: 'post',
        body: formData
      }).then(res => res.json())
      .then(json => {
        if (json.success === true){
          this.$router.push('/login')
        }
      })
    }
  },
}
</script>

<style scoped>

</style>
<template>
  <div>
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-lg-4">
          <h1 class="my-4 text-center">Login Form</h1>
          <form action="" @submit.prevent="login" ref="loginForm">
            <div class="form-floating mb-3">
              <input type="email" class="form-control" id="email" name="email" placeholder="name@example.com">
              <label for="email">Email address</label>
            </div>
            <div class="form-floating mb-3">
              <input type="password" class="form-control" id="password" name="password" placeholder="Password">
              <label for="password">Password</label>
            </div>
            <div class="mb-3">
              <button class="btn btn-primary"><i class="bi bi-box-arrow-in-right"></i> Login</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {mapGetters, mapState} from "vuex";
import axios from "axios";

export default {
  computed: {
    ...mapState([
      'auth',
      'token'
    ]),
    ...mapGetters([
      'getUrl'
    ])
  },
  methods: {
    login() {
      let formData = new FormData(this.$refs.loginForm);
      fetch(this.getUrl('/login'),{
        method: 'post',
        body: formData
      }).then(res => res.json())
          .then(json => {
            if (json.success === true){
              localStorage.setItem('auth',JSON.stringify(json.auth))
              localStorage.setItem('token',json.token)

              this.$store.dispatch('setAuth',json.auth)
              this.$store.dispatch('setToken',json.token)
              axios.defaults.headers.common['Authorization'] = "Bearer "+localStorage.getItem('token');

              this.$router.push('/dashboard')
            }
          })
    }
  },
}
</script>

<style scoped>

</style>
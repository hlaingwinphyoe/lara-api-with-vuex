<template>
  <nav class="navbar navbar-expand-lg bg-primary text-white shadow navbar-dark">
    <div class="container">
      <router-link to="/" class="navbar-brand text-white">Laravel Vue</router-link>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse " id="navbarSupportedContent">
        <ul class="navbar-nav ms-auto mb-2 mb-lg-0 text-white">
          <li class="nav-item">
            <router-link class="nav-link" to="/">Home</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/about">About</router-link>
          </li>
          <template v-if="auth === null">
            <li class="nav-item">
              <router-link class="nav-link" to="/login">Login</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/register">Register</router-link>
            </li>
          </template>
          <template v-else>
            <li class="nav-item">
              <router-link class="nav-link" to="/dashboard">Dashboard</router-link>
            </li>
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                Products
              </a>
              <ul class="dropdown-menu dropdown-menu-dark">
                <li><router-link class="dropdown-item" to="/products/create"><i class="bi bi-plus"></i> Add Product</router-link></li>
                <li><router-link class="dropdown-item" to="/products"><i class="bi bi-boxes"></i> Products List</router-link></li>
              </ul>
            </li>
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                {{ auth.name }}
              </a>
              <ul class="dropdown-menu dropdown-menu-dark">
                <li><a class="dropdown-item" href="#">Action</a></li>
                <li><a class="dropdown-item" href="#">Another action</a></li>
                <li><hr class="dropdown-divider"></li>
                <li>
                  <a class="dropdown-item" href="#" @click="logout()">
                    <i class="bi bi-box-arrow-right"></i> Log Out
                  </a>
                </li>
              </ul>
            </li>

          </template>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
import {mapGetters, mapState} from "vuex";

export default {
  name: "NavBar",
  computed: {
    ...mapState([
        "auth",
        "token"
    ]),
    ...mapGetters([
        "getUrl"
    ])
  },
  methods: {
    logout() {
      localStorage.removeItem('auth');
      localStorage.removeItem('token');
      let headers = new Headers();
      headers.append("Authorization", "Bearer "+this.token);
      fetch(this.getUrl('/logout'),{
        method: "post",
        headers
      }).then(res => res.json())
      .then(json => {
        if (json.success === true){
          this.$store.dispatch('logout')
          this.$router.push('/login')
        }
      })
    }
  },
}
</script>

<style scoped>

</style>
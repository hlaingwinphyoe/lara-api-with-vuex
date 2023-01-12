<template>
  <nav>
    <ul class="pagination">
      <li v-for="link in links" class="page-item" :class="{'disabled':!link.url}">
        <a class="page-link" @click.prevent="go(link.url)" :class="{'active':link.active}" href="#">{{ link.label }}</a>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: "Pagination",
  props: {
    links: {
      type: Array,
    },
  },
  methods: {
    go(url) {
      this.$emit('fetchLink',url)
    },
    changeName(){
      this.links[0].label = "Prev";
      this.links[this.links.length-1].label = "Next";
    }
  },
  mounted() {
    this.changeName()
  },
  updated() {
    this.changeName()
  }
}
</script>

<style scoped>

</style>
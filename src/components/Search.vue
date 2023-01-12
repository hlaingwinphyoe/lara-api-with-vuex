<template>
  <input type="text" v-model="keyword" class="form-control" placeholder="Search">
</template>

<script>
import {throttle} from "lodash";

export default {
  name: "Search",
  data() {
    return {
      keyword: null
    }
  },
  methods: {
    // search() {
    //   this.$emit('search',this.keyword)
    // }
  },
  watch: {
    // x is new value & y is old value
    keyword:throttle(function (){
      this.$emit('search',this.keyword)
    },500)
  }
}
</script>

<style scoped>

</style>
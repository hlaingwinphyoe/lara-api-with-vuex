<template>
  <div class="form-floating mb-3">
    <input :type="type" :name="name" class="form-control" :value="value" :class="{'is-invalid':errors[name]}" :id="name" :placeholder="name">
    <label :for="name">{{ label }}</label>
  </div>
  <div class="invalid-feedback" v-if="errors[name]">{{ errors[name][0] }}</div>
</template>

<script>
export default {
  name: "Input",
  props: {
    label: {
      type: String,
      default: "Input Label"
    },
    type: {
      type: String,
      default: "text"
    },
    name: {
      type: String,
      default: "input_name"
    },
    errors: {
      type: Object
    },
    value: {
      default: null
    }
  },
}
</script>

<style scoped>

</style>
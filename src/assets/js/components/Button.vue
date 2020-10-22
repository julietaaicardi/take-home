<template>
  <button :type="type" v-text="text"/>
</template>

<script>
export default {
  name: 'Button',
  props: {
    text: {
      type: String,
      default: ''
    },
    type: {
      type: String,
      default: 'submit'
    },
  },
}
</script>

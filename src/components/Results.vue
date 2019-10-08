<template>
  <div class="results-wrapper">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'Results',
  props: {
    msg: String,
  },
}
</script>

<style scoped lang="scss"></style>

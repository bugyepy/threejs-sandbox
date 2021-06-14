<template lang="pug">
  .container
    CreateModel
</template>

<script lang="ts">
import { defineComponent, ref } from '@nuxtjs/composition-api'

export default defineComponent({
  setup() {
    const text = ref('タイトル')

    return {
      text,
    }
  },
})
</script>

<style lang="scss">
.container {
  margin: 0 auto;
  // min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>

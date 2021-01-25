<template>
  <div class="home">
    <Box />
  </div>
</template>

<script>
import Box from '@/components/Box.vue'

export default {
  name: 'Home',
  components: {
    Box
  }
}
</script>

<style lang="scss" scoped>
.home {
  display: flex;
  height: 100vh;
  width: 100%;
  flex-flow: column;
  justify-content: center;
  align-items: center;
}
</style>

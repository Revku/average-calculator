<template>
  <div class="home">
    <Box />
    <p class="home__footer">
      <a href="https://github.com/TheRevQ/average-calculator" target="_blank">Open source</a> by TheRevQ
    </p>
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

  &__footer {
    color: white;
    margin-top: 20px;

    a {
      color: white;
    }

    margin-bottom: 20px;
  }
}
</style>

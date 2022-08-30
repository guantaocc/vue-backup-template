<template>
  <Ellipsis :content="'this is a content'" class="demo" />
</template>

<script>
import Ellipsis from '@/components/Ellipsis'
export default {
  components: {
    Ellipsis
  }
}
</script>

<style lang="scss" scoped>
.demo {
  color: red;
}
</style>

<template>
  <div class="home">
    <DragonList />
  </div>
</template>

<script>
// @ is an alias to /src
import DragonList from '@/components/DragonList.vue'

export default {
  name: 'Home',
  data() {
    return {
      api: this.$parent.api
    }
  },
  components: {
    DragonList
  }
}
</script>

<style lang="scss">
  @import '../styles/variables';
  @import '../styles/mixins';
</style>
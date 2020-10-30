<template>
  <div id="app">
    <div class="nav">
      <div class="max-area nav-flex">
        <router-link class="nav-link" to="/"><img :src="origin + '/img/logo/logo.svg'" height="64"><h2 class="hdn">Menu</h2></router-link>
        <div>
          <router-link class="nav-link" to="/">Dragon List</router-link> |
          <router-link class="nav-link" to="/user-area">Creation Area</router-link>
        </div>
      </div>
    </div>

    <router-view/>
  </div>
</template>

<script>
// @ is an alias to /src
import DragonList from '@/components/DragonList.vue'

export default {
  data() {
    return {
      origin: window.location.origin,
      api: 'http://5c4b2a47aa8ee500142b4887.mockapi.io/api/v1/dragon/'
    }
  }
}
</script>


<style lang="scss">
@import 'styles/variables';
@import 'styles/mixins';
@import 'styles/structure';

@import 'styles/master';
</style>

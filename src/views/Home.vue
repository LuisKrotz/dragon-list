<template>
  <div :class="'home ' + (!loaded ? 'loading': '')">
    <DragonList :dragons="dragons"/>
  </div>
</template>

<script>
// @ is an alias to /src
import DragonList from '@/components/DragonList.vue'

export default {
  name: 'Home',
  components: {
    DragonList
  },
  data() {
    return {
      dragons: [],
      loaded: false
    }
  },
  mounted() {
    this.getPost();
  },
  methods: {
    getPost() {
      let self = this;

      fetch(`${self.$parent.api}/dragon`)
        .then((response) => {
          return response.json();
        }).then((data) => {
          self.dragons = data;
          self.loaded = true;
        });
    }
  }
}
</script>

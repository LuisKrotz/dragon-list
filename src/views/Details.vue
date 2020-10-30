<template>
    <div :class="'max-area ' + (!loaded ? 'loading': '')">
      <div class="dragon-details">
          <h2 class="dragon-details-title">Dragon Details</h2>
          <div>
            <h3 class="dragon-details-label">Name: </h3>
            <p class="dragon-details-item"> {{ dragon.name }}</p>
          </div>
          <div>
            <h3 class="dragon-details-label">Type: </h3>
            <p class="dragon-details-item"> {{ dragon.type }}</p>
          </div>
          <div>
            <h4 class="dragon-details-label">Creation: </h4>
            <p class="dragon-details-item"> {{ dragon.createdAt }}</p>
          </div>
      </div>
    </div>
</template>

<script>
export default {
  name: 'DragonList',
  data() {
    return {
      dragon: [],
      loaded: false
    }
  },
  mounted() {
    const urlParams = new URLSearchParams(window.location.search);
    const id = urlParams.get('id');
    let self = this;

    self.updateDragonList(id);
  },
  methods: {
    updateDragonList(id) {
      let self = this, ye, mo, da, createdAt;

      self.loaded = false;

      fetch(`${self.$parent.api}${id}`)
        .then((response) => {
          return response.json();
        }).then((data) => {
          self.dragon = data;

          createdAt = new Date(self.dragon.createdAt);
          ye = new Intl.DateTimeFormat('en', { year: 'numeric' }).format(createdAt),
          mo = new Intl.DateTimeFormat('en', { month: 'short' }).format(createdAt),
          da = new Intl.DateTimeFormat('en', { day: '2-digit' }).format(createdAt);

          self.dragon.createdAt = `${mo} ${da}  / ${ye}`;
          self.loaded = true;
        });
    },
  }
}
</script>


<style lang="scss">
  @import '../styles/variables';
  @import '../styles/mixins';

  @import '../styles/details';
</style>


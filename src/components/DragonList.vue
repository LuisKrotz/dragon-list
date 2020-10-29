<template>
  <ul class="hello">
    <li v-for="dragon in dragons" :key="dragon.id">
      {{ dragon.name }} / {{ dragon.type }}
    </li>
  </ul>
</template>

<script>
export default {
  name: 'DragonList',
  props: {
    dragons: {
      type: Array,
      required: true
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: sblock;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

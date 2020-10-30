<template>
    <div :class="'max-area ' + (!loaded ? 'loading': '')">
        <div :class="'dragon-list ' + (!crud ? 'col-2' : 'col-3') ">
            <div class="dragon-list-item header">
              <div class="dragon-list-header">Dragon Name</div>
              <div class="dragon-list-header">Dragon Type</div>
              <div class="dragon-list-header" v-if="crud">Options</div>
            </div>
          <div class="dragon-list-item" v-if="crud">
              <div class="dragon-list-cell">
                <label class="dragon-list-label" for="dragon_name_create">Dragon Name: </label>
                <input class="dragon-list-input" type="text" ref="dragon_name_create" name="dragon_name_create" placeholder="New dragon: name">
              </div>
              <div class="dragon-list-cell">
                <label class="dragon-list-label" for="dragon_type_create">Dragon Type: </label>
                <input class="dragon-list-input" type="text" ref="dragon_type_create" name="dragon_type_create" placeholder="New dragon: Type">
              </div>
              <div class="dragon-list-cell create" v-if="crud">
                <button class="dragon-list-create" ref="dragon_create" @click="updateDragon(false)">Create Dragon</button>
              </div>
            </div>

            <div class="dragon-list-scrollable">
              <div class="dragon-list-item" v-for="dragon in dragons" :key="dragon.id">
                <div class="dragon-list-cell">
                  <label class="dragon-list-label" :for="'dragon_name_edit' + dragon.id">Name: </label>
                  <router-link :to="'/dragon?id=' + dragon.id" v-if="!crud" class="dragon-list-link">{{ dragon.name }}</router-link>
                  <input v-else class="dragon-list-input" type="text" ref="dragon_name_edit" name="dragon_name_edit" :data-name-edit="dragon.id" :id="'dragon_name_edit' + dragon.id" :value="dragon.name">
                </div>
                <div class="dragon-list-cell">
                  <label class="dragon-list-label" :for="'dragon_type_edit' + dragon.id">Type: </label>
                  <router-link v-if="!crud"  :to="'/dragon?id=' + dragon.id" class="dragon-list-link">{{ dragon.type }}</router-link>
                  <input v-else class="dragon-list-input" type="text" ref="dragon_type_edit" name="dragon_type_edit" :data-type-edit="dragon.id" :id="'dragon_type_edit' + dragon.id" :value="dragon.type">
                </div>
                <div class="dragon-list-cell buttons" v-if="crud">
                  <button class="dragon-list-edit" ref="dragon_edit" @click="updateDragon(dragon.id)">Edit</button>
                  <button class="dragon-list-delete" ref="dragon_remove" @click="removeDragon(dragon.id)">Remove</button>
                </div>
              </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'DragonList',
  data() {
    return {
      dragons: [],
      loaded: false
    }
  },
  props: {
    crud: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  mounted() {
    let self = this;

    self.updateDragonList();
  },
  methods: {
    updateDragon(id) {
      let self = this,
          data;

      let api = `${self.$parent.api}`,
          action = 'POST';

      if (id) {
        api += `${id}`,
        action = 'PUT',
        data = {
          name: self.$el.querySelectorAll('[data-name-edit="' + id +'"]')[0].value,
          type: self.$el.querySelectorAll('[data-type-edit="' + id +'"]')[0].value
        };
      }

      else {
       data = {
          name: self.$refs.dragon_name_create.value,
          type: self.$refs.dragon_type_create.value
        };
      }

      fetch(api, {
        method: action,
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
      }).then((response) => {
        response.json().then((response) => {
          self.updateDragonList();
        })
      }).catch(err => {
        console.error(err)
      });
    },
    removeDragon(id) {
      let self = this;

      fetch(`${self.$parent.api}${id}`, {
        method: 'DELETE'
      }).then((response) => {
        self.updateDragonList();
      }).catch(err => {
        console.error(err);
      });
    },
    updateDragonList() {
      let self = this;

      self.loaded = false;

      fetch(`${self.$parent.api}`)
        .then((response) => {
          return response.json();
        }).then((data) => {
          self.dragons = data.reverse();
          self.loaded = true;
        });
    },
  }
}
</script>


<style lang="scss">
  @import '../styles/variables';
  @import '../styles/mixins';

  @import '../styles/components/DragonList';
</style>


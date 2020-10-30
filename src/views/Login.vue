<template>
  <div class="max-area">
    <form class="login" @submit.prevent="submit" v-if="!logged">
      <div class="login-field">
        <label class="login-label" for="user_name">User Name</label>
        <input class="login-input" ref="user_name" type="text" name="user_name" id="user_name" @focus="clear"/>
      </div>

      <div class="login-field">
        <label class="login-label" for="password">Password</label>
        <input class="login-input" ref="user_password" type="password" name="user_password" id="user_password" @focus="clear"/>
        <span v-if="error" class="login-error" >Can't log in, please check your credentials</span>
      </div>

      <button class="login-submit" input type="submit">Login</button>
      <button class="login-reset" type="button">Forgot my Password</button>
    </form>
    <DragonList v-else :crud="crud"/>
  </div>
</template>


<script>
// import Vue from 'vue'
// import VueCryptojs from 'vue-cryptojs'
 
// Vue.use(VueCryptojs)

// @ is an alias to /src
import DragonList from '@/components/DragonList.vue'

export default {
  name: 'Login',
  components: {
    DragonList
  },
  data() {
    return {
      origin: window.location.origin,
      key: '1234567890123456',
      logged: false,
      error: false,
      crud: true,
      api: this.$parent.api
    }
  },
  methods: {
    testPassword(passwords) {
      let self = this, found = false;
      let user = self.$refs.user_name.value,
          userpassword = self.$refs.user_password.value;

      passwords.forEach(function (password) {
        // if (self.$refs.user_name.value === password.user && password === self.CryptoJS.AES.decrypt(password.password, self.key).toString(self.CryptoJS.enc.Utf8))

        if (self.$refs.user_name.value === password.user && userpassword === password.password)
           found = true;
      });

      return found;
    },
    clear() {
      this.error = false;
    },
    submit() {
      let self = this;

      self.error = false;

      fetch(`${self.origin}/json/passwords.json`)
        .then((response) => {
          return response.json();
        }).then((data) => {
          if (self.testPassword(data)) {
            self.logged = true;
          } else {
            self.error = true;
          }
        });
    }
  }
}
</script>

<style lang="scss">
  @import '../styles/variables';
  @import '../styles/mixins';

  @import '../styles/login';
</style>

<template>
  <div id="app">

    <nav class="navbar is-light">
      <div class="navbar-menu is-active">
        <div class="navbar-end">
          <a class="navbar-item" @click="switchSign('sign-in')">Войти</a>
          <a class="navbar-item" @click="switchSign('sign-up')">Зарегистрироваться</a>
        </div>
      </div>
    </nav>

    <section class="section">
      <div class="container">
        <sign-in v-if="sign === 'sign-in'"></sign-in>
        <sign-up v-else></sign-up>
      </div>
    </section>

  </div>
</template>

<script>
  import SignIn from './components/SignIn.vue'
  import SignUp from './components/SignUp.vue'

  export default {
    name: 'app',

    data () {
      return {
        sign: 'sign-in'
      }
    },

    components: {
      SignIn,
      SignUp
    },

    methods: {
      switchSign(currentSign) {
        this.sign = currentSign;
      }
    }
  }
</script>

<style lang="scss" scoped>

</style>

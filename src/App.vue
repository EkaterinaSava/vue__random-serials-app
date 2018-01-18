<template>
  <div id="app">

    <nav class="navbar is-light">
      <div class="navbar-menu is-active">
        <div class="navbar-end" v-if="!signComplete">
          <a class="navbar-item" @click="switchSign('sign-in')">Войти</a>
          <a class="navbar-item" @click="switchSign('sign-up')">Зарегистрироваться</a>
        </div>
        <div class="navbar-end" v-else>
          <span class="navbar-email">{{ email }}</span>
        </div>
      </div>
    </nav>

    <section class="section" v-if="!isMainPage">
      <div class="container">
        <sign-in v-if="sign === 'sign-in'" @addUser="isMainPage = $event.mainPage, signComplete = $event.complete, email = $event.email, uid = $event.uid"></sign-in>
        <sign-up v-else @regSuccess="sign = $event"></sign-up>
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
        sign: 'sign-in',
        isMainPage: false,
        signComplete: false,
        email: '',
        uid: ''
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
  .navbar-email {
    align-self: center;
    padding: 0 20px;
  }
</style>

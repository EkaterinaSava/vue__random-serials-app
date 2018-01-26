<template>
  <div id="app">
    <nav class="navbar is-light">
      <div class="navbar-menu is-active">
        <div class="navbar-end" v-if="!signComplete">
          <router-link to="/sign-in" class="navbar-btn">
            <button type="button" class="button is-primary" @click="switchSign('sign-in')">Войти</button>
          </router-link>
          <router-link to="/sign-up" class="navbar-btn">
            <button type="button" class="button is-primary is-outlined" @click="switchSign('sign-up')">Зарегистрироваться</button>
          </router-link>
        </div>
        <div class="navbar-end" v-else>
          <router-link to="/serials" class="navbar-btn">
            <button type="button" class="button is-info">Список сериалов</button>
          </router-link>
          <span class="navbar-email">{{ email }}</span>
        </div>
      </div>
    </nav>

    <section class="section">
      <router-view @addUser="email = $event.email, signComplete = $event.signComplete, userUid = $event.uid" :uid="userUid"></router-view>
    </section>

  </div>
</template>

<script>
  import HelloPage from './components/HelloPage.vue'
  import SignIn    from './components/SignIn.vue'
  import SignUp    from './components/SignUp.vue'
  import MainPage  from './components/MainPage.vue'

  export default {
    name: 'app',

    data () {
      return {
        signComplete: false,
        email: '',
        uid: '',
        userUid: ''
      }
    },

    components: {
      HelloPage,
      SignIn,
      SignUp,
      MainPage
    },

    methods: {
      switchSign(currentSign) {
        this.sign = currentSign;
      }
    }
  }
</script>

<style lang="scss" scoped>
  .button {
    transition: all .25s ease-out;
  }
  .navbar {
    &-btn {
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 5px;
    }

    &-email {
      align-self: center;
      padding: 0 20px;
    }
  }
</style>

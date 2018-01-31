<template>
  <div id="app">
    <nav class="navbar is-light">
      <div class="navbar-menu is-active">
        <div class="container">
          <div class="navbar-end" v-if="!signComplete">
            <router-link to="/sign-in" class="navbar-btn">
              <button type="button" class="button is-link" @click="switchSign('sign-in')">Войти</button>
            </router-link>
            <router-link to="/sign-up" class="navbar-btn">
              <button type="button" class="button is-link is-outlined" @click="switchSign('sign-up')">Зарегистрироваться</button>
            </router-link>
          </div>
          <div class="navbar-end" v-else>
            <router-link to="/serials" class="navbar-btn">
              <button type="button" class="button is-link">Список сериалов</button>
            </router-link>
            <span class="navbar-email">{{ email }}</span>
          </div>
        </div>
      </div>
    </nav>

    <section class="section">
      <div class="container">
          <router-view @addUser="email = $event.email, signComplete = $event.signComplete, userUid = $event.uid" :uid="userUid"></router-view>
      </div>
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

<!-- Общие стили для всего приложения -->
<style lang="scss">
  body {
    min-width: 320px;
  }

  .button {
    padding-left: 1.25em;
    padding-right: 1.25em;
    transition: all .25s ease-out;
  }

  .navbar {
    &-menu {
      box-shadow: 0 5px 10px rgba(10,10,10,.1);
    }
    &-end {
      display: flex;
      height: 100%;
      justify-content: flex-end;
      margin-left: auto
    }
    &-btn {
      display: flex;
      justify-content: center;
      align-items: center;
      margin-left: 20px;
    }
    &-email {
      align-self: center;
      padding: 0 20px;
    }
  }

  @media screen and (max-width: 1023px) {
    .navbar-end {
      padding: 0 1.5rem;
    }
  }

  .box {
    max-width: 100%;
  }

  .content {
    word-break: break-word;
  }

</style>

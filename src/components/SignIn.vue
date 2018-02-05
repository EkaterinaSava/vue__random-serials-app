<template>
  <div class="page__auth page__auth--sign-in">
    <div class="columns">
      <div class="column is-4 is-offset-4">
        <h3 class="title is-3">Вход</h3>

        <form class="" action="" method="" @submit.prevent="loginUser" v-if="show" class="page__auth-form">
          <div class="field">
            <label class="label">E-mail</label>
            <div class="control">
              <input id="signin-email" class="input" type="email" placeholder="Ваша электронная почта" required v-model="user.email">
            </div>
          </div>

          <div class="field">
            <label class="label">Пароль</label>
            <div class="control">
              <input id="signin-pass" class="input" type="password" placeholder="Ваш пароль" required v-model="user.password">
            </div>
          </div>

          <div class="control">
            <button class="button is-link" type="submit">Войти</button>
          </div>
        </form>

        <!-- если вход прошел успешно -->
        <div v-if="signSuccess" class="page__auth-form-success">
          <div class="notification is-success">
            <strong>Поздравляем!</strong><br>
            Вы успешно вошли.
          </div>
          <p>Теперь вы можете воспользоваться кнопкой «Список сериалов» ниже или в шапке страницы.</p>
          <router-link to="/serials" class="page__auth-form-serials">
            <button type="button" class="button is-link">Список сериалов</button>
          </router-link>
        </div>

        <!-- а если нет -->
        <div v-if="signError" class="page__auth-form-error">
          <div class="notification is-danger">
            <strong>Внимание!</strong><br>
            Такой пары email-password нет в нашей базе.<br>
            Проверьте правильность введенных данных.
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'sign-in',

    data() {
      return {
        show: true,
        signSuccess: false,
        signError: false,

        user: {
          email: '',
          password: ''
        }
      }
    },

    methods: {
      loginUser() {
        // по аналогии с регистрацией
        firebase.auth().signInWithEmailAndPassword(this.user.email, this.user.password)
          // если все прошло успешно, то:
          .then( response => {
            //console.log(response);
            // нам нужны: email юзера, чтобы показать его в верхнем правом углу, его уникальный ID (ключ)
            // плюс, если все прошло успешно - надо перекинуть юзера на главную страницу с формы входа (mainPage: true)
            // плюс передать, что вход успешен complete: true
            const sett = {
              email: response.email,
              uid: response.uid,
              mainPage: true,
              signComplete: true
            }
            this.$emit('addUser', sett);
            this.show = false;
            this.signSuccess = true;
          })
          .catch(error => {
            this.signError = true;
          })
      }
    }
  }
</script>

<style lang="scss" scoped>

  .page {
    &__auth {
      &-form {
        &-serials {
          display: block;
          margin-top: 20px;
        }
      }
    }
  }

</style>

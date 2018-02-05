<template>
  <div class="page__auth page__auth--sign-up">
    <div class="columns">
      <div class="column is-4 is-offset-4">
        <h3 class="title is-3">Регистрация</h3>
        <form class="" action="" method="" @submit.prevent="registerUser" v-if="show" class="page__auth-form">
          <div class="field">
            <label class="label">E-mail</label>
            <div class="control">
              <input id="signup-email" class="input" type="email" placeholder="Ваша электронная почта" required v-model="user.email">
            </div>
          </div>

          <div class="field">
            <label class="label">Пароль (не менее 6 символов)</label>
            <div class="control">
              <input id="signup-pass" class="input" type="password" placeholder="Придумайте пароль" required v-model="user.password">
            </div>
          </div>

          <div class="field">
            <label class="label">Повторите пароль</label>
            <div class="control">
              <input id="signup-pass2" class="input" type="password" placeholder="Повторите пароль" required v-model="user.confirmPassword">
            </div>
          </div>

          <div class="notification is-danger" v-if="errorConfirm">
            <strong>Внимание!</strong><br>
            Регистрация не была завершена, так как введенные пароли не совпадают.
          </div>

          <div class="notification is-danger" v-if="errorSmall">
            <strong>Внимание!</strong><br>
            Регистрация не была завершена, так как пароль короче 6 символов.
          </div>

          <div class="control">
            <button class="button is-link" type="submit">Зарегистрироваться</button>
          </div>
        </form>

        <!-- если регистрация прошла успешно -->
        <div v-if="signSuccess" class="page__auth-form-success">
          <div class="notification is-success">
            <strong>Поздравляем!</strong><br>
            Вы успешно зарегистрировались.
          </div>
          <p>Теперь вы можете воспользоваться кнопкой «Войти» ниже или в шапке страницы.</p>
          <router-link to="/sign-in" class="page__auth-form-signin">
            <button type="button" class="button is-link" @click="switchSign('sign-in')">Войти</button>
          </router-link>
        </div>

        <!-- а если нет -->
        <div v-if="signError" class="page__auth-form-error">
          <div class="notification is-danger">
            <strong>Внимание!</strong><br>
            Введенные данные содержат ошибку.
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'sign-up',

    data() {
      return {
        show: true,
        signSuccess: false,
        signError: false,

        user: {
          email: '',
          password: '',
          confirmPassword: ''
        },

        errorConfirm: false,
        errorSmall: false
      }
    },

    methods: {
      registerUser() {
        this.errorConfirm = false;
        this.errorSmall = false;
        // так как все поля имеют атрибут required то проверку для инпутов мы не используем
        // но нам надо проверить совпадают ли пароли и если нет, вывести нотификейшн об этом
        if ( this.user.password !== this.user.confirmPassword) {
          this.errorConfirm = true;
        }
        else if ( this.user.password.length < 6 ) {
          this.errorSmall = true;
        }
        else {
          // это то, что мы будем передавать в App.vue для дальнейших действий
          firebase.auth().createUserWithEmailAndPassword(this.user.email, this.user.password)
            // если юзер будет успешно создан, будет выполнена эта команда
            // буквально - здесь мы передаем данные родительскому компоненту
            .then( () => {
              // мы должны будем повесить событие с именем первого аргумента на компонент <sign-up> в App.vue
              // в эвенте, которое будет получать это событие, будет храниться строка 'sign-in'
              this.$emit('regSuccess', 'sign-in');
              this.show = false;
              this.signSuccess = true;
            })
            .catch( error => {
              this.signError = true;
            })
        }
      }
    }
  }
</script>

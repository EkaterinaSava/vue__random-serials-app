<template>
  <div class="page__hello">
    <div class="tile is-ancestor">

      <div class="tile is-7 is-vertical is-parent">
        <div class="tile is-child box">
          <div class="content">
            <p class="title">Привет!</p>
            <p>Я — <strong>небольшое демонтрационное приложение</strong>, которое позволяет:</p>
            <ul>
              <li>создать список сериалов</li>
              <li>выбрать из него рандомный сериал для просмотра</li>
              <li>если вы уже смотрели этот сериал — можно пометить его как просмотренный</li>
            </ul>
          </div>
        </div>

        <div class="tile is-child box">
          <div class="content">
            <p class="title">Вы можете...</p>
            <p>...зарегистрироваться, чтобы затем иметь сохраненный список сериалов.</p>
            <p>...при регистрации использовать любой email, так как подтверждение электронной почты не требуется.</p>
          </div>
        </div>
      </div>

      <div class="tile is-5 is-parent">
        <div class="tile is-child box">
          <div class="content">
            <p class="title">«Обо мне»</p>
            <p><strong>Под капотом:</strong></p>
            <ul>
              <li>vue.js</li>
              <li>via vue-cli (simple webpack)</li>
              <li>vue-router</li>
              <li>firebase</li>
              <li>bulma (css framework)</li>
            </ul>
            <br>
            <p><strong>Репозиторий</strong> — <a class="link" href="https://github.com/EkaterinaSava/vue__random-serials-app" target="_blank">vue: random-serials-app</a></p>
            <p>В
              <a class="link" href="https://github.com/EkaterinaSava/vue__random-serials-app/blob/master/README.md" target="_blank">Readme</a>
              вы найдете описание принципов работы приложения и основного функционала :)
            </p>
            <br>
            <p>:octocat:</p>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
  export default {
    name: 'hello-page'
  }
</script>

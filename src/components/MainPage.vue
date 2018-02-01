<template>
  <div class="page__main">

    <div class="tile is-ancestor">
      <div class="tile is-parent">
        <article class="tile is-child box">
          <div class="content">
            <ol class="serials__list">
              <li v-for="(serial, index) in data.serials" :key="serial" class="serials__list-item">
                <span class="serials__list-item-inner">
                  <span>{{ serial }}</span>
                  <a class="delete is-small" @click="removeSerial(index)"></a>
                </span>
              </li>
            </ol>
          </div>
          <button class="button is-link is-medium" type="submit" @click="chooseRandomSerial(0, data.serials.length)">Выбрать случайный сериал</button>
        </article>
      </div>
      <div class="tile is-parent">
        <article class="tile is-child box">
          <form class="" action="" method="">
            <label class="label">Добавить новый сериал в список</label>
            <div class="field">
              <div class="control">
                <input class="input" type="text" placeholder="Введите название сериала" v-model="title">
              </div>
            </div>
            <div class="field is-grouped is-grouped-right">
              <div class="control">
                <button class="button is-dark" type="submit" @click.prevent="addSerial">Добавить в список</button>
              </div>
            </div>
          </form>
        </article>
      </div>
    </div>

    <div class="notification box">
      {{ data.currentSerial }}
    </div>

    <div class="field is-grouped is-grouped-right">
      <div class="control">
        <button class="button is-dark is-outlined" type="submit" @click="removeCurrentSerial">Этот сериал уже просмотрен! Удалить из списка</button>
      </div>
    </div>

  </div>
</template>

<script>
  export default {
    name: 'main-page',

    data() {
      return {
        title: '',

        data: {
          serials: [],
          currentSerial: 'Сериал не выбран...',
          numberOfCurrentSerial: null
        }
      }
    },

    props: ['uid'],

    methods: {
      addSerial() {
        if (this.title) {
          this.data.serials.push(this.title);
        }

        firebase.database().ref('users/' + this.uid).set({
          data: this.data
        });

        this.title = '';
      },

      removeSerial(index) {
        this.data.serials.splice(index, 1);

        if ( this.data.numberOfCurrentSerial === index ) {
          this.data.currentSerial = 'Сериал не выбран...';
          this.data.numberOfCurrentSerial = null;
        }

        for (let i = 0; i < this.data.serials.length; i++) {
          if ( this.data.currentSerial === this.data.serials[i] ) {
            this.data.numberOfCurrentSerial = i;
          }
        }

        firebase.database().ref('users/' + this.uid).set({
          data: this.data
        });
      },

      chooseRandomSerial(min, max) {
        // просто типичный рандом
        const random = Math.floor(Math.random() * (max - min) + min);

        // если в списке есть хотя бы 1 сериал
        if ( this.data.serials.length > 0 ) {
          this.data.currentSerial = this.data.serials[random];
          this.data.numberOfCurrentSerial = random;
        }

        firebase.database().ref('users/' + this.uid).set({
          data: this.data
        });
      },

      removeCurrentSerial() {
        // удалить можно только тот сериал, который был выбран
        if ( this.data.numberOfCurrentSerial !== null && this.data.numberOfCurrentSerial ) {
          // удаляем
          this.data.serials.splice(this.data.numberOfCurrentSerial, 1);
          // заменяем его название на строку текста
          this.data.currentSerial = 'Сериал не выбран...';
          // номер текущего сериала становится 'null'
          this.data.numberOfCurrentSerial = null;
        }

        firebase.database().ref('users/' + this.uid).set({
          data: this.data
        });
      }

    },

    created() {
      this.data.serials = [];

      const takeSerials = firebase.database().ref('users/' + this.uid + '/data');

      takeSerials.on('value', (snapshot) => {
        if ( snapshot.val().currentSerial && snapshot.val().serials ) {
          this.data = snapshot.val();
        }
        else {
          this.data.serials = [];
          this.data.currentSerial = 'Сериал не выбран...';
        }
      });
    }
  }
</script>

<style lang="scss" scoped>
  .serials {
    &__list {
      &-item {
        &-inner {
          display: inline-block;
          line-height: 20px;
          margin-bottom: 5px;
        }
        & .delete {
          display: inline-block;
          margin-left: 5px;
          margin-top: 2px;
        }
      }
    }
  }
</style>

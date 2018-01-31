<template>
  <div class="page__main">


    <div class="tile is-ancestor">
      <div class="tile is-parent">
        <article class="tile is-child box">
          <div class="content">
            <ol>
              <li v-for="(serial, index) in data.serials" :key="serial" @click="removeSerial(index)">{{ serial }}</li>
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
        <button class="button is-dark is-outlined" type="submit" @click="removeCurrentSerial">Этот сериал уже просмотрен!</button>
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

        for (let i = 0; i < this.data.serial.length; i++) {
          if ( this.data.currentSerial === this.data.serials[i] ) {
            this.data.numberOfCurrentSerial = i;
          }
        }

        firebase.database().ref('users/' + this.uid).set({
          data: this.data
        });
      },

      chooseRandomSerial(min, max) {
        const random = Math.floor(Math.random() * (max - min) + min);

        if ( this.data.serials.length > 0 ) {
          this.data.currentSerial = this.data.serials[random];
          this.data.numberOfCurrentSerial = random;
        }

        firebase.database().ref('users/' + this.uid).set({
          data: this.data
        });
      },

      removeCurrentSerial() {
        if ( this.data.numberOfCurrentSerial !== null && this.data.numberOfCurrentSerial ) {
          this.data.serials.splice(this.data.numberOfCurrentSerial, i);
          this.data.numberOfCurrentSerial = 'Сериал не выбран...';
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

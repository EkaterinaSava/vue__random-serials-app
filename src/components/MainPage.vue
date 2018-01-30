<template>
  <div class="page__main">

    <form class="" action="" method="">
      <label class="label">Введите название любого сериала</label>
      <div class="field has-addons">
        <div class="control">
          <input class="input" type="text" placeholder="Сериал" v-model="title">
        </div>
        <div class="control">
          <button class="button is-info" type="submit" @click.prevent="addSerial">Добавить в список</button>
        </div>
      </div>
    </form>

    <br>

    <ol>
      <li v-for="(serial, index) in data.serials" :key="serial" @click="removeSerial(index)">{{ serial }}</li>
    </ol>

    <br>

    <div>
      <button class="button is-info" type="submit" @click="chooseRandomSerial(0, data.serials.length)">Выбрать случайный сериал</button>

      <br><br>

      <div class="notification">
        {{ data.currentSerial }}
      </div>

      <button class="button is-success" type="submit" @click="removeCurrentSerial">Этот сериал уже просмотрен!</button>
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

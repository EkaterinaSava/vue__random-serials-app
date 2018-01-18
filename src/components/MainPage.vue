<template>
  <div class="main-page">

    <form class="" action="" method="">
      <label class="label">Введите название любого сериала</label>
      <div class="field has-addons">
        <div class="control">
          <input class="input" type="text" placeholder="Сериал">
        </div>
        <div class="control">
          <button class="button is-info" type="submit">Добавить в список</button>
        </div>
      </div>
    </form>

    <br>

    <ol>

    </ol>

    <br>

    <div>
      <button class="button is-info" type="submit">Выбрать случайный сериал</button>

      <br><br>

      <div class="notification">
        Primar lorem ipsum dolor sit amet, consectetur
        adipiscing elit lorem ipsum dolor. <strong>Pellentesque risus mi</strong>, tempus quis placerat ut, porta nec nulla. Vestibulum rhoncus ac ex sit amet fringilla. Nullam gravida purus diam, et dictum <a>felis venenatis</a> efficitur. Sit amet,
        consectetur adipiscing elit
      </div>

      <button class="button is-success" type="submit">Этот сериал уже просмотрен!</button>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'main-page'
  }
</script>

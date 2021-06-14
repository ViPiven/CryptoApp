<template>
  <div class="quotes">
    <div class="quotes__panel">
      <h2 class="quotes__title">Панель котировок</h2>
      <div class="quotes__data">
        <div class="quotes__subtitles">
          <h3 class="quotes__subtitle">Валютные пары</h3>
          <h3 class="quotes__subtitle">Изменения</h3>
          <h3 class="quotes__subtitle">Цена</h3>
        </div>
        <ul class="quotes__list">
          <li
              class="quotes__item"
              v-for="currencyPair in currencyPairs"
              :key="currencyPair"
          >
            <h2>{{ currencyPair.pair.join(' | ') }}</h2>
            <h2>%</h2>
            <h2>{{ currencyPair.price }}</h2>
          </li>
        </ul>
      </div>
    </div>
    <Diagram
        :getPrise="getPrise"
        :prise="price"
    />
  </div>
</template>

<script>

import Diagram from "@/components/Diagram";
export default {
  name: "Quotes",
  props: ['price', 'getPrise', 'runWebSocket', 'currencyPairs'],
  components: {Diagram},
  methods: {

  },

  created() {
    this.runWebSocket()
  }
}
</script>

<style scoped lang="scss">
.quotes {
  height: 100vh;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  background-color: #000;

  &__panel {
    grid-column-start: 1;
    grid-column-end: 2;
  }

  &__title {
    display: flex;
    align-items: center;
    font-family: Montserrat,sans-serif;
    font-weight: 600;
    font-size: 24px;
    line-height: 150%;
    text-transform: uppercase;
    color: #fff;
    background-color: #191A20;
    padding: 22px 0 22px 35px;
    margin-bottom: 5px;
  }

  &__data {
    height: 100vh;
    padding: 20px 50px 0 40px;
    background-color: #191A20;
    color: #fff;
  }

  &__subtitles {
    display: flex;
    justify-content: space-between;
  }

  &__subtitle {
    font-family: Montserrat,sans-serif;
    font-style: normal;
    font-weight: normal;
    font-size: 16px;
    line-height: 150%;
    color: #757679;
  }

  &__list {
    list-style-type: none;
  }

  &__item {
    display: flex;
    justify-content: space-between;
  }
}
</style>
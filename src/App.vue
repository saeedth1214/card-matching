<script>
import Card from "./components/card.vue";

export default {
  components: {
    Card,
  },

  setup() {
    const cardList = [];

    for (let index = 0; index < 16; index++) {
      cardList.push(index);
    }

    return { cardList };
  },
};
</script>

<template>
  <h1>Peek-a-Vue</h1>

  <section class="game-board">
    <Card
      v-for="(card, index) in cardList"
      :key="`card-${index}`"
      :value="card"
    />
  </section>
</template>

<style>
#app {
  text-align: center;
  margin-top: 60px;
  font-family: Arial, Helvetica, sans-serif;
}

.card {
  border: 3px solid #ccc;
}
.game-board {
  display: grid;
  grid-template-columns: 100px 100px 100px 100px;
  grid-template-rows: 100px 100px 100px 100px;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
  justify-content: center;
}
</style>

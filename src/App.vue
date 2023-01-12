<script>
import Card from "./components/card.vue";
import { ref } from "vue";
export default {
  components: {
    Card,
  },

  setup() {
    const cardList = ref([]);

    const flipCard = (payload) => {
      cardList.value[payload.position].visible =
        !cardList.value[payload.position].visible;
    };

    for (let index = 0; index < 16; index++) {
      cardList.value.push({
        value: index,
        visible: false,
        position: index,
      });
    }

    return { cardList, flipCard };
  },
};
</script>

<template>
  <h1>Peek-a-Vue</h1>

  <section class="game-board">
    <Card
      v-for="(card, index) in cardList"
      :key="`card-${index}`"
      :value="card.value"
      :visible="card.visible"
      :position="card.position"
      @select-card="flipCard"
    />
  </section>
</template>

<style>
#app {
  text-align: center;
  margin-top: 60px;
  font-family: Arial, Helvetica, sans-serif;
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

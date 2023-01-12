<script>
import Card from "./components/card.vue";
import { ref, watch, computed } from "vue";
import _ from "lodash";

export default {
  components: {
    Card,
  },

  setup() {
    const cardList = ref([]);
    const userSelection = ref([]);
    const status = computed(() => {
      if (remainingPaires.value === 0) {
        return "Player Wins.";
      } else {
        return `Remining paires : ${remainingPaires.value}`;
      }
    });

    const remainingPaires = computed(() => {
      const remainingCards = cardList.value.filter(
        (card) => card.match === false
      ).length;
      return remainingCards / 2;
    });

    const shuffleCards = () => {
      cardList.value = _.shuffle(cardList.value);
    };

    const flipCard = (payload) => {
      cardList.value[payload.position].visible =
        !cardList.value[payload.position].visible;

      if (userSelection.value[0]) {
        userSelection.value[1] = payload;
      } else {
        userSelection.value[0] = payload;
      }
    };

    for (let index = 0; index < 16; index++) {
      cardList.value.push({
        value: index,
        visible: false,
        position: index,
        match: false,
      });
    }

    watch(
      userSelection,
      (currentValue) => {
        if (currentValue.length === 2) {
          const cardOne = currentValue[0];
          const cardTwo = currentValue[1];

          if (cardOne.faceValue === cardTwo.faceValue) {
            status.value = "Match!";
            cardList.value[cardOne.position].match = true;
            cardList.value[cardTwo.position].match = true;
          } else {
            status.value = "MisMatch!";

            cardList.value[cardOne.position].visible = false;
            cardList.value[cardTwo.position].visible = false;
          }
          currentValue.length = 0;
        }
      },
      { deep: true }
    );

    return { cardList, flipCard, status, remainingPaires, shuffleCards };
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
      :match="card.match"
      @select-card="flipCard"
    />
  </section>
  <h2>{{ status }}</h2>
  <button @click="shuffleCards">Shuffle Cards</button>
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

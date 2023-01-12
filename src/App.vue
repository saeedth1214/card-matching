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

    const restartGame = () => {
      shuffleCards();

      cardList.value = cardList.value.map((card, index) => {
        return {
          ...card,
          visible: false,
          match: false,
          position: index,
        };
      });
    };

    const shuffleCards = () => {
      cardList.value = _.shuffle(cardList.value);
    };

    const flipCard = (payload) => {
      cardList.value[payload.position].visible = true;
      // !cardList.value[payload.position].visible;

      if (userSelection.value[0]) {
        userSelection.value[1] = payload;
      } else {
        userSelection.value[0] = payload;
      }
    };

    const cardItems = [
      "bat",
      "candy",
      "cauldron",
      "cupcake",
      "ghost",
      "moon",
      "pumpkin",
      "witch-hat",
    ];

    cardItems.forEach((item) => {
      cardList.value.push({
        value: item,
        visible: false,
        position: null,
        match: false,
      });
      cardList.value.push({
        value: item,
        visible: false,
        position: null,
        match: false,
      });
    });

    cardList.value = cardList.value.map((card, index) => {
      return {
        ...card,
        position: index,
      };
    });

    watch(
      userSelection,
      (currentValue) => {
        if (currentValue.length === 2) {
          const cardOne = currentValue[0];
          const cardTwo = currentValue[1];

          if (cardOne.position === cardTwo.position) {
            cardList.value[cardOne.position].visible = false;
            currentValue.length = 0;
            return;
          }

          if (cardOne.faceValue === cardTwo.faceValue) {
            cardList.value[cardOne.position].match = true;
            cardList.value[cardTwo.position].match = true;
          } else {
            setTimeout(() => {
              cardList.value[cardOne.position].visible = false;
              cardList.value[cardTwo.position].visible = false;
            }, 2000);
          }
          currentValue.length = 0;
        }
      },
      { deep: true }
    );

    return { cardList, flipCard, status, remainingPaires, restartGame };
  },
};
</script>

<template>
  <h1 class="sr-only">Peek-a-Vue</h1>

  <img src="/images/peek-a-vue-title.png" alt="peek-a-vue" class="title" />
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
  <button @click="restartGame" class="button">
    <img
      src="/images/restart.svg"
      style="margin-right: 0.5rem"
      alt="Restart icon"
    />
    Restart Game
  </button>
</template>

<style>
body,
html {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}
h1 {
  margin-top: 0px;
}
#app {
  text-align: center;
  padding-top: 15px;
  color: white;
  font-family: Avenir, Helvetica, sans-serif;
  background-image: url("/images/page-bg.png");
  background-color: #00070c;
  height: 100vh;
}
.game-board {
  display: grid;
  grid-template-columns: repeat(4, 100px);
  grid-template-rows: repeat(4, 100px);
  grid-column-gap: 15px;
  grid-row-gap: 15px;
  justify-content: center;
}

.title {
  margin-bottom: 10px;
}
.button {
  background-color: orange;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0.75rem 0.5rem;
  color: white;
  border-radius: 0.5rem;
  margin: 0px auto;
  border: none;
  font-weight: bold;
}
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
</style>

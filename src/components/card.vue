<script>
import { computed } from "vue";
export default {
  props: {
    position: {
      type: Number,
      required: true,
    },
    value: {
      type: String,
      required: true,
    },

    visible: {
      type: Boolean,
      default: false,
    },
    match: {
      type: Boolean,
      default: false,
    },
  },

  setup(props, context) {
    const selectCard = () => {
      context.emit("selectCard", {
        position: props.position,
        faceValue: props.value,
      });
    };

    const flippedStyles = computed(() => {
      if (props.visible) {
        return "is-flipped";
      }
    });

    return { selectCard, flippedStyles };
  },
};
</script>
<template>
  <div class="card" @click="selectCard" :class="flippedStyles">
    <div class="card-face is-front">
      <img :src="`/images/${value}.png`" style="width: 100px" :alt="value" />
      <img src="/images/checkmark.svg" v-if="match" class="icon-checkmark" />
    </div>
    <div class="card-face is-back"></div>
  </div>
</template>
<style scoped>
.card {
  position: relative;
  cursor: pointer;
  transition: transform 0.3s ease-in;
  transform-style: preserve-3d;
}

.card.is-flipped {
  transform: rotateY(180deg);
}

.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  backface-visibility: hidden;
}
.card-face.is-front {
  background-image: url("/images/card-bg.png");
  color: white;
  transform: rotateY(180deg);
}

.card-face.is-back {
  background-image: url("/images/card-bg-empty.png");
}
.icon-checkmark {
  position: absolute;
  right: 5px;
  bottom: 5px;
}
</style>

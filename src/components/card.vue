<script>
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

    return { selectCard };
  },
};
</script>
<template>
  <div class="card" @click="selectCard">
    <div v-if="visible" class="card-face is-front">
      <img :src="`/images/${value}.png`" style="width: 100px;" :alt="value" />
      <img src="/images/checkmark.svg" v-if="match" class="icon-checkmark" />
    </div>
    <div v-else class="card-face is-back">
      <h3>{{ position + 1 }}</h3>
    </div>
  </div>
</template>
<style scoped>
.card {
  position: relative;

  cursor: pointer;
}

.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.card-face.is-front {
  background-image: url("/images/card-bg.png");
  color: white;
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

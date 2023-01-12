<script>
export default {
  props: {
    position: {
      type: Number,
      required: true,
    },
    value: {
      type: Number,
      required: true,
    },

    visible: {
      type: Boolean,
      required: false,
    },
  },

  setup(props, context) {
    const selectCard = () => {
      context.emit("selectCard", {
        position: props.value,
      });
    };

    return { selectCard };
  },
};
</script>
<template>
  <div class="card" @click="selectCard">
    <div v-if="visible" class="card-face is-front">
      {{ value }}
    </div>
    <div v-else class="card-face is-back">back</div>
  </div>
</template>
<style scoped>
.card {
  border: 3px solid #ccc;
  position: relative;

  cursor: pointer;
}

.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
}
.card-face.is-front {
  background-color: red;
  color: white;
}

.card-face.is-back {
  background-color: blue;
  color: white;
}
</style>

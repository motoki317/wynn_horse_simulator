<template>
  <div>
    <div id="grid-container">
      <div class="grid-item" id="title">Horse</div>
      <div class="grid-item"></div>
      <div class="grid-item top-bar">Current</div>
      <div class="grid-item top-bar">Target</div>

      <div class="grid-item horse-name">Brown</div>
      <img class="grid-item" src="../assets/brown.png" alt="brown" />
      <div class="grid-item">{{ current.brown }}</div>
      <input type="number" v-model="target.brown" class="grid-item" />

      <div class="grid-item horse-name">Black</div>
      <img class="grid-item" src="../assets/black.png" alt="brown" />
      <div class="grid-item">{{ current.black }}</div>
      <input type="number" v-model="target.black" class="grid-item" />

      <div class="grid-item horse-name">Chestnut</div>
      <img class="grid-item" src="../assets/chestnut.png" alt="brown" />
      <div class="grid-item">{{ current.chestnut }}</div>
      <input type="number" v-model="target.chestnut" class="grid-item" />

      <div class="grid-item horse-name">White</div>
      <img class="grid-item" src="../assets/white.png" alt="brown" />
      <div class="grid-item">{{ current.white }}</div>
      <input type="number" v-model="target.white" class="grid-item" />
    </div>
  </div>
</template>

<script>
export default {
  name: "HorseView",
  props: {
    current: {
      brown: Number,
      black: Number,
      chestnut: Number,
      white: Number
    },
    onTargetChange: Function
  },
  data: function() {
    return {
      target: {
        brown: 0,
        black: 0,
        chestnut: 0,
        white: 0
      }
    };
  }
};
</script>

<style scoped>
#grid-container {
  display: grid;
  grid-template-columns: 150px 75px 150px 150px;
  grid-template-rows: 50px 75px 75px 75px 75px;
  margin: 15px 60px;
}

#title {
  font-weight: bolder;
  font-size: 30px;
  margin: auto;
}

.top-bar {
  font-weight: bold;
}

.horse-name {
  font-weight: bold;
}

img {
  image-rendering: pixelated;
  width: 64px;
  display: block;
  margin: 0 auto 0;
}

.grid-item {
  font-size: 25px;
  vertical-align: middle;
  margin: auto;
}

input[type="number"] {
  -moz-appearance: textfield;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

input {
  border: 0;
  -webkit-appearance: none;
  height: auto;
  margin-bottom: 38px;
  text-align: center;
  width: 100px;
  color: inherit;
  font: inherit;
}
</style>

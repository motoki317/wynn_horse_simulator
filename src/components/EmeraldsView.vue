<template>
  <div>
    <div id="grid-container">
      <div class="grid-item" id="title">Emeralds</div>
      <div class="grid-item" id="stacks-container">
        <img
          src="../assets/experience_bottle.png"
          alt="experience_bottle"
          class="bottle-top"
        />
        <img
          src="../assets/experience_bottle.png"
          alt="experience_bottle"
          class="bottle-bottom-1"
        />
        <img
          src="../assets/experience_bottle.png"
          alt="experience_bottle"
          class="bottle-bottom-2"
        />
      </div>
      <div class="grid-item"></div>
      <img
        src="../assets/experience_bottle.png"
        alt="experience_bottle"
        class="grid-item"
      />
      <div class="grid-item"></div>
      <img
        src="../assets/emerald_block.png"
        alt="emerald_block"
        class="grid-item"
      />
      <div class="grid-item"></div>

      <span class="grid-item left-bar">Budget</span>
      <input
        class="grid-item"
        type="number"
        v-model="budgetStacks"
        @change="() => onBudgetChange(this.budget)"
      />
      <span class="grid-item unit">Stx</span>
      <input
        class="grid-item"
        type="number"
        v-model="budgetLE"
        @change="() => onBudgetChange(this.budget)"
      />
      <span class="grid-item unit">LE</span>
      <input
        class="grid-item"
        type="number"
        v-model="budgetEB"
        @change="() => onBudgetChange(this.budget)"
      />
      <span class="grid-item unit">EB</span>

      <span class="grid-item left-bar">Spent</span>
      <span class="grid-item">{{ spentStacks }}</span>
      <span class="grid-item unit">Stx</span>
      <span class="grid-item">{{ spentLE }}</span>
      <span class="grid-item unit">LE</span>
      <span class="grid-item">{{ spentEB }}</span>
      <span class="grid-item unit">EB</span>
    </div>
  </div>
</template>

<script>
function toStacks(emeralds) {
  return Math.floor(emeralds / 64 / 64);
}

function toLE(emeralds) {
  return Math.floor(emeralds / 64) - toStacks(emeralds) * 64;
}

function toEB(emeralds) {
  return emeralds - toStacks(emeralds) * 64 * 64 - toLE(emeralds) * 64;
}

export default {
  name: "HorseView",
  props: {
    onBudgetChange: Function,
    spent: Number
  },
  data: function() {
    return {
      budgetStacks: 0,
      budgetLE: 0,
      budgetEB: 0
    };
  },
  computed: {
    budget() {
      return this.budgetStacks * 64 * 64 + this.budgetLE * 64 + this.budgetEB;
    },
    spentStacks() {
      return toStacks(this.spent);
    },
    spentLE() {
      return toLE(this.spent);
    },
    spentEB() {
      return toEB(this.spent);
    }
  }
};
</script>

<style scoped>
#grid-container {
  display: grid;
  grid-template-columns: 150px 100px 50px 100px 50px 100px 50px;
  grid-template-rows: 75px 75px 75px;
  grid-row-gap: 10px;
  margin: 15px;
}

#title {
  font-weight: bolder;
  font-size: 30px;
  margin: auto;
}

.left-bar {
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

.unit {
  margin: auto 0;
  text-align: left;
}

#stacks-container {
  position: relative;
  width: 64px;
  height: 64px;
}

.bottle-top {
  position: absolute;
  top: 16px;
  left: 16px;
  z-index: 1;
  width: 48px;
}

.bottle-bottom-1 {
  position: absolute;
  top: 0;
  left: 8px;
  z-index: 0;
  width: 40px;
}

.bottle-bottom-2 {
  position: absolute;
  top: 0;
  left: 32px;
  z-index: 0;
  width: 40px;
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
  margin-bottom: 35px;
  text-align: center;
  width: 75px;
  color: inherit;
  font: inherit;
}
</style>

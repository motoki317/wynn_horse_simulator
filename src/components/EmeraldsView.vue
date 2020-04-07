<template>
  <div>
    <div id="grid-container">
      <div class="grid-item" id="title">Emeralds</div>
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
      <img src="../assets/emerald.png" alt="emerald" class="grid-item" />
      <div class="grid-item"></div>

      <span class="grid-item left-bar">Budget</span>
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
      <input
        class="grid-item"
        type="number"
        v-model="budgetE"
        @change="() => onBudgetChange(this.budget)"
      />
      <span class="grid-item unit">E</span>

      <span class="grid-item left-bar">Spent</span>
      <span class="grid-item">{{ spentLE }}</span>
      <span class="grid-item unit">LE</span>
      <span class="grid-item">{{ spentEB }}</span>
      <span class="grid-item unit">EB</span>
      <span class="grid-item">{{ spentE }}</span>
      <span class="grid-item unit">E</span>
    </div>
  </div>
</template>

<script>
function toLE(emeralds) {
  return Math.floor(emeralds / 64 / 64);
}

function toEB(emeralds) {
  return Math.floor(emeralds / 64) - toLE(emeralds) * 64;
}

function toE(emeralds) {
  return emeralds - toLE(emeralds) * 64 * 64 - toEB(emeralds) * 64;
}

export default {
  name: "HorseView",
  props: {
    onBudgetChange: Function,
    spent: Number
  },
  data: function() {
    return {
      budgetLE: 0,
      budgetEB: 0,
      budgetE: 0
    };
  },
  computed: {
    budget() {
      return this.budgetLE * 64 * 64 + this.budgetEB * 64 + this.budgetE;
    },
    spentLE() {
      return toLE(this.spent);
    },
    spentEB() {
      return toEB(this.spent);
    },
    spentE() {
      return toE(this.spent);
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
  text-align: left;
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

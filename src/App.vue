<template>
  <div id="app">
    <h1 id="title">Wynncraft Horse Breeding Simulator</h1>

    <p>
      Input your budget and target horse numbers, and click "Start Breeding"!
    </p>
    <p>
      To specify no budget or no target, leave them at 0.
    </p>

    <button :disabled="isBreeding" @click="() => this.startBreeding()">
      Start Breeding
    </button>
    <button :disabled="!isBreeding" @click="() => this.endBreeding()">
      Stop Breeding
    </button>

    <Interval
      :on-interval-change="newInterval => (this.interval = newInterval)"
    ></Interval>

    <div id="views">
      <EmeraldsView
        id="emeralds-view"
        :on-budget-change="newBudget => (this.budget = newBudget)"
        :spent="spent"
      ></EmeraldsView>
      <HorseView
        id="horse-view"
        :current="current"
        :on-target-change="newTargets => (this.target = newTargets)"
      ></HorseView>
    </div>
  </div>
</template>

<script>
import EmeraldsView from "./components/EmeraldsView.vue";
import HorseView from "./components/HorseView";
import Interval from "./components/Interval";

export default {
  name: "App",
  components: {
    EmeraldsView,
    HorseView,
    Interval
  },
  data: function() {
    return {
      isBreeding: false,
      // breeding interval in ms
      interval: 100,
      // budget and spent money in EB
      budget: 0,
      spent: 0,
      // current and target horse amounts
      current: {
        brown: 0,
        black: 0,
        chestnut: 0,
        white: 0
      },
      target: {
        brown: 0,
        black: 0,
        chestnut: 0,
        white: 0
      }
    };
  },
  methods: {
    startBreeding() {
      this.isBreeding = true;
    },
    endBreeding() {
      this.isBreeding = false;
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 100%;
}

button {
  background-color: #0062ac;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 0 5px;
  cursor: pointer;
}

button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

button:hover:not(:disabled) {
  background-color: #004dac;
}

button:focus {
  outline: 0;
}

#title {
  margin: 50px auto;
  font-size: 32px;
}

#emeralds-view {
  margin: 50px auto 0;
  width: 650px;
  height: 300px;
  border: 1px #888 solid;
  border-radius: 5px;
}

#horse-view {
  margin: 50px auto 0;
  width: 650px;
  height: 400px;
  border: 1px #888 solid;
  border-radius: 5px;
}
</style>

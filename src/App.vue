<template>
  <div id="app">
    <h1 id="title">Wynncraft Horse Breeding Simulator</h1>

    <p>
      Input your budget and target horse numbers, and click "Start Breeding"!
    </p>
    <p>
      To specify no budget or no target, leave them at 0.
    </p>

    <p>
      <button :disabled="isBreeding" @click="() => this.startBreeding()">
        Start Breeding
      </button>
      <button :disabled="!isBreeding" @click="() => this.endBreeding()">
        Stop Breeding
      </button>
    </p>

    <p>
      <button @click="() => this.reset()">
        Reset
      </button>
    </p>

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

    <p class="credit">
      <a href="https://github.com/motoki317/wynn_horse_simulator"
        >GitHub repository</a
      >
    </p>
    <p class="credit">
      Made by <a href="https://forums.wynncraft.com/members/32964/">motoki1</a>,
      images are from <a href="https://wynncraft.com">Wynncraft</a> and
      <a href="https://forums.wynncraft.com/threads/210764/"
        >Wynn Improvements</a
      >.
    </p>
  </div>
</template>

<script>
import EmeraldsView from "./components/EmeraldsView.vue";
import HorseView from "./components/HorseView";
import Interval from "./components/Interval";

// 24 EB
const brownHorsePrice = 24;

const higherTierChance = 0.2;
const sameTierChance = 0.5;
// lower tier chance = 0.3;

// Returns 1 if success, 0 if same tier, -1 if lower tier
function breedResult() {
  const res = Math.random();
  if (res <= higherTierChance) {
    return 1;
  } else if (res <= higherTierChance + sameTierChance) {
    return 0;
  } else {
    return -1;
  }
}

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
      this.breedOnce();
    },
    endBreeding() {
      this.isBreeding = false;
    },
    reset() {
      this.isBreeding = false;
      this.spent = 0;
      this.current = {
        brown: 0,
        black: 0,
        chestnut: 0,
        white: 0
      };
    },
    hasReachedTarget() {
      const allTargetZero =
        this.target.brown <= 0 &&
        this.target.black <= 0 &&
        this.target.chestnut <= 0 &&
        this.target.white <= 0;

      return (
        !allTargetZero &&
        (this.target.brown <= 0 || this.target.brown <= this.current.brown) &&
        (this.target.black <= 0 || this.target.black <= this.current.black) &&
        (this.target.chestnut <= 0 ||
          this.target.chestnut <= this.current.chestnut) &&
        (this.target.white <= 0 || this.target.white <= this.current.white)
      );
    },
    breedOnce() {
      if (!this.isBreeding) {
        return;
      }

      // Check if reached all target numbers
      if (this.hasReachedTarget()) {
        this.isBreeding = false;
        return;
      }

      const allTargetZero =
        this.target.brown <= 0 &&
        this.target.black <= 0 &&
        this.target.chestnut <= 0 &&
        this.target.white <= 0;
      const toBreedChestnut =
        allTargetZero ||
        (this.target.white > 0 && this.target.white > this.current.white);
      const toBreedBlack =
        allTargetZero ||
        (this.target.chestnut > 0 &&
          this.target.chestnut > this.current.chestnut);
      const toBreedBrown =
        allTargetZero ||
        (this.target.black > 0 && this.target.black > this.current.black);

      // Breed chestnut -> white
      if (toBreedChestnut && this.current.chestnut >= 2) {
        const res = breedResult();
        if (res === 1) {
          this.current.chestnut -= 2;
          this.current.white++;
        } else if (res === 0) {
          this.current.chestnut--;
        } else {
          this.current.chestnut -= 2;
          this.current.black++;
        }
      } else if (
        // Breed black -> chestnut
        (toBreedChestnut || toBreedBlack) &&
        this.current.black >= 2
      ) {
        const res = breedResult();
        if (res === 1) {
          this.current.black -= 2;
          this.current.chestnut++;
        } else if (res === 0) {
          this.current.black--;
        } else {
          this.current.black -= 2;
          this.current.brown++;
        }
      } else if (
        // Breed brown -> black
        (toBreedChestnut || toBreedBlack || toBreedBrown) &&
        this.current.brown >= 2
      ) {
        const res = breedResult();
        if (res === 1) {
          this.current.brown -= 2;
          this.current.black++;
        } else {
          this.current.brown--;
        }
      } else {
        // Buy brown
        if (this.budget > 0 && this.budget <= this.spent + brownHorsePrice) {
          // Used all budget
          this.isBreeding = false;
          return;
        }

        this.current.brown++;
        this.spent += brownHorsePrice;
      }

      setTimeout(() => this.breedOnce(), this.interval);
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

.credit {
  margin: 15px;
  font-size: 12px;
  opacity: 0.8;
  text-align: right;
}
</style>

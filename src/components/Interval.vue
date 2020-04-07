<template>
  <div>
    <p>Breeding Interval: {{ interval }} ms</p>
    <p class="slider-container">
      <label for="interval">
        <span>{{ minInterval }} ms</span>
        <input
          type="range"
          :min="minInput"
          :max="maxInput"
          class="slider"
          id="interval"
          v-model="intervalInput"
          @change="() => this.onIntervalChange(this.interval)"
        />
        <span>{{ maxInterval }} ms</span>
      </label>
    </p>
  </div>
</template>

<script>
const minInterval = 5;
const maxInterval = 1000;

const minInput = 0;
const maxInput = 1000;

export default {
  name: "Interval",
  props: {
    onIntervalChange: Function
  },
  data: function() {
    return {
      minInterval: minInterval,
      maxInterval: maxInterval,
      minInput: minInput,
      maxInput: maxInput,
      intervalInput: Math.round((minInput + maxInput) / 2)
    };
  },
  computed: {
    // to log scale
    interval() {
      const minLog = Math.log(minInterval);
      const maxLog = Math.log(maxInterval);

      const scale = (maxLog - minLog) / (maxInput - minInput);

      return Math.round(
        Math.exp(minLog + scale * (this.intervalInput - minInput))
      );
    }
  }
};
</script>

<style>
.slider-container {
  width: 60%;
  margin: auto;
}

.slider {
  -webkit-appearance: none;
  width: 80%;
  height: 15px;
  border-radius: 5px;
  background: #d3d3d3;
  outline: none;
  opacity: 0.7;
  -webkit-transition: 0.2s;
  transition: opacity 0.2s;
}

.slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 25px;
  height: 25px;
  border-radius: 50%;
  background: #4caf50;
  cursor: pointer;
}

.slider::-moz-range-thumb {
  width: 25px;
  height: 25px;
  border-radius: 50%;
  background: #4caf50;
  cursor: pointer;
}
</style>

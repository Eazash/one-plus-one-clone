<template>
  <div class="progress-bar-wrapper">
    <div class="progress-bar" :style="{ background: color, width: width }">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "progress-bar",
  props: {
    max: [String, Number],
    current: [String, Number]
  },
  computed: {
    percentage: function () {
      return Math.floor((this.current / this.max) * 100);
    },
    width: function () {
      return this.percentage + "%";
    },
    color: function () {
      return this.percentage > 50
        ? "#42b983"
        : this.percentage > 25
          ? "#ecd71d"
          : this.percentage > 10
            ? "#ff5e00"
            : "#f44336";
    }
  }
}
</script>

<style>
.progress-bar-wrapper {
  width: 95%;
  padding: 10px 5px;
  margin: auto;
  background: #e4e4e4;
}
.progress-bar {
  width: 100%;
  height: 10px;
  margin: 0 !important;
  text-align: center;
}
</style>
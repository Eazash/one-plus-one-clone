<template>
  <div class="answer">
    <p v-text="number" @click="selected"></p>
  </div>
</template>

<script>
export default {
  props: {
    number: { type: Number, required: true },
  },
  methods: {
    selected() {
      this.$emit('chosen', this.number);
    }
  }
};
</script>

<style>
</style>

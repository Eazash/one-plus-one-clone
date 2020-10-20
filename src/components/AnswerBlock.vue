<template>
  <div class="answer" @click="selected">
    <p v-text="number"></p>
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

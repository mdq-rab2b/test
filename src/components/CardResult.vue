<template>
  <div class="card-result">
    <text><slot></slot></text>
    <span class="score">
      {{ data.score }}
      <span class="flag" v-if="data.score == 'AA Large'">🟧</span>
      <span class="flag" v-if="data.score == 'Fail'">🟥</span>
    </span>
    <span class="ratio">{{ data.ratio }}</span>
    <span class="bg" :style="`background-color: ${bg}`">
      <span class="dot" :style="`background-color: ${color}`"></span>
    </span>
  </div>
</template>

<script>
export default {
  name: 'MyComponent',
  props: {
    data: null,
    color: null,
    bg: null,
  }
}
</script>

<style lang="stylus" scoped>
@import "./CardResult"
</style>
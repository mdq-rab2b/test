<template>
  <div class="gallery">
    <div v-for="theme in themes" :key="theme.name">
      <h2>{{ theme.name }}</h2>
      <span class="col">

        <CardResult
          :data="theme.foreground_test"
          :color="theme.foreground"
          :bg="theme.background">
          Foreground:
        </CardResult>

        <CardResult
          v-for="index in 16"
          :key="index"
          :data="theme[`color_${String(index).padStart(2, '0')}_test`]"
          :color="theme[`color_${String(index).padStart(2, '0')}`]"
          :bg="theme.background">
          Color {{ String(index).padStart(2, '0') }}:
        </CardResult>

      </span>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import contrast from "get-contrast";

export default {
  data() {
    return {
      themes: []
    };
  },
  mounted() {
    axios
      .get('https://raw.githubusercontent.com/Gogh-Co/Gogh/master/data/themes.json')
      .then(response => {
        this.themes = response.data.themes.map(theme => {
          const colors = {};
          Object.keys(theme).forEach(key => {
            if (key.startsWith('color_')) {
              colors[`${key}_test`] = this.testColorContrast(theme.background, theme[key]);
            }
          });
          return {
            ...theme,
            foreground_test: this.testColorContrast(theme.background, theme.foreground),
            ...colors,
          };
        });
      })
    .catch(error => {
      console.error(error);
    });
  },
  methods: {
    getThemeStyles(theme) {
      return {
        backgroundColor: theme.background,
        color: theme.foreground
      };
    },
    testColorContrast(color1, color2) {
      return {
        score: contrast.score(color1, color2),
        ratio: contrast.ratio(color1, color2).toFixed(2)
      }
    },
  }
};
</script>

<style lang="stylus">
@import "app"
</style>

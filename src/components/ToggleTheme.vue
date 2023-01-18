<template>
  <a
    role="button"
    @click.prevent="toggleTheme()"
    :aria-label="'Toggle ' + nextTheme"
    :title="'Toggle ' + nextTheme"
    class="toggle-theme nes-pointer"
  >
    <img v-bind:src="'/images/8bit_sun.png'" class="theme-img nes-pointer" v-if="theme === 'light'">
    <img v-bind:src="'/images/8bit_moon.png'" class="theme-img nes-pointer" v-if="theme === 'dark'">
  </a>
</template>

<script>
let themes = ["light", "dark"];
export default {
  data() {
    return {
      theme: "dark",
    };
  },
  computed: {
    nextTheme() {
      const currentIndex = themes.indexOf(this.theme);
      const nextIndex = (currentIndex + 1) % themes.length;
      return themes[nextIndex];
    },
  },
  methods: {
    toggleTheme() {
      const currentIndex = themes.indexOf(this.theme);
      const nextIndex = (currentIndex + 1) % themes.length;
      window.__setPreferredTheme(themes[nextIndex]);
      this.theme = themes[nextIndex];
      this.$theme == this.theme;
      this.$disqus && this.$disqus.reset();
    },
  },
  async mounted() {
    // set default
    if (typeof window.__theme !== "undefined") this.theme = window.__theme;
  },
};
</script>

<style>
.toggle-theme {
  background-color: transparent;
  border: 0;
  color: currentColor;
  cursor: pointer;
}

.toggle-theme:hover {
  opacity: 0.8;
  color: var(--border-color);
}

.toggle-theme:focus {
  outline: none;
}

.toggle-theme:hover {
  background-color: transparent;
}

.theme-img {
  height: 2rem;
  width: auto;
}
</style>

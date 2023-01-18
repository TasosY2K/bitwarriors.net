<template>
  <a
    role="button"
    @click.prevent="toggleFlicker()"
    :aria-label="'Toggle Flicker ' + flicker"
    :title="'Toggle Flicker ' + flicker"
    class="toggle-flicker nes-pointer"
  >
    <img v-bind:src="'/images/8bit_flicker_off.png'" class="flicker-img nes-pointer" v-if="flicker === 'on'">
    <img v-bind:src="'/images/8bit_flicker_on.png'" class="flicker-img nes-pointer" v-if="flicker === 'off'">
  </a>
</template>

<script>

export default {
  data() {
    return {
      flicker: "off",
    };
  },
  methods: {
    toggleFlicker: function() {
      if (this.flicker === "on") {
        localStorage.setItem("flicker", "off")
        this.flicker = "off";
      } else if (this.flicker == "off") {
        localStorage.setItem("flicker", "on")
        this.flicker = "on";
      }
      this.$emit("forward-flicker", this.flicker)
    },
  },
  mounted() {
    let storedFlicker = localStorage.getItem("flicker");
    if (storedFlicker == "on") {
      this.flicker = "on"
    } else if (storedFlicker == "off") {
      this.flicker = "off"
    }
  }
};
</script>

<style>
.toggle-flicker {
  background-color: transparent;
  border: 0;
  color: currentColor;
  cursor: pointer;
}

.toggle-flicker:click {
  animation: none !important;
}

.toggle-flicker:hover {
  opacity: 0.8;
  color: var(--border-color);
}

.toggle-flicker:focus {
  outline: none;
}

.toggle-flicker:hover {
  background-color: transparent;
}

.flicker-img {
  margin-left: 4px;
  height: 2rem;
  width: auto;
}
</style>

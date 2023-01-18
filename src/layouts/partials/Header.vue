<template>
  <div>
    <header class="layout-header">
      <nav class="nav">
        <ToggleTheme />
        <ToggleFlicker @forward-flicker="forwardFlicker" />
      </nav>
    </header>
  </div>
</template>

<script>
import ToggleTheme from "@/components/ToggleTheme";
import ToggleFlicker from "@/components/ToggleFlicker";

export default {
  components: {
    ToggleTheme,
    ToggleFlicker
  },
  methods: {
    forwardFlicker: function(flicker) {
      this.$emit("forward-flicker", flicker)
    }
  }
};
</script>

<style>
.layout-header {
  width: 100%;
  margin: 0px auto;
}
.nav {
  display: flex;
  justify-content: flex-end;
}
</style>
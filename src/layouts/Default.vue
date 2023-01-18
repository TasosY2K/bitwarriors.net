<template>
  <div v-bind:class="flicker === 'on' ? '' : 'crt'">
    <main class="layout" role="main">
      <Header @forward-flicker="handleFlicker" />
      <slot />
      <div class="footer">
        <span>
          <p><b><span class="green">[</span>Site<span class="green">]</span></b>&nbsp;<a target="_blank" href="https://bitwarriors.net">https://bitwarriors.net</a></p>
          <p style="margin: 0; padding-bottom: 2rem"><span><b><span class="green">[</span>Twitter<span class="green">]</span></b></span>&nbsp;<a target="_blank" href="https://twitter.com/BitWarriorsgr">https://twitter.com/BitWarriorsgr</a></p>
        </span>
      </div>
    </main>
  </div>
</template>

<script>
import "~/assets/css/nes.min.css";
import "~/assets/css/crt.css";
import Header from "./partials/Header";

export default {
  components: {
    Header,
  },
  data() {
    return {
      flicker: "off"
    };
  },
  methods: {
    handleFlicker: function(flicker) {
      this.flicker = flicker
    }
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
@font-face {
  font-family: Retro Gaming;
  src: url("../../static/fonts/Retro Gaming.ttf");
}

:root {
  --text-color: #333;
  --border-color: #fecd43;
  --bkg-color: white;
  --line-color: #cdc8c5;
}

:root [data-theme="dark"] {
  --text-color: #ffffff;
  --border-color: #fecd43;
  --bkg-color: #030200;
  --line-color: #fecd43;
}

body {
  font-family: 'Retro Gaming';
  margin: 0;
  padding: 0;
  line-height: 1.5;
  background-color: var(--bkg-color);
  color: var(--text-color);
  transition: background 0.3s;
}

a {
  color: #03be1c;
  text-decoration: underline;
}

img {
    width:100%;
    max-width:1024px;
}
code {
  font-family: 'Retro Gaming';
  background-color: #03be1c;
  color: black;
  border-radius: 5px;
  font-size: inherit;
  padding: 5px;
}

a:hover {
  color: #030200;
  text-decoration: none;
  background-color: #03be1c;
}

.nes-btn {
  text-decoration: none;
}

.nes-btn.is-success {
  background-color: #00dd1d;
}

.nes-btn.is-success:hover {
  background-color: #01ce1c;
}

.nes-btn.is-success:focus {
  box-shadow: none;
}

.layout {
  min-height: 100vh;
  padding-top: 30px;
  max-width: 1100px;
  margin: 0 auto;
  height: 100%;
  font-family: 'Retro Gaming';
  padding-left: 20px;
  padding-right: 20px;
}

.footer {
  text-align: left !important;
  margin-top: 3rem;
  text-align: center;
  font-size: 1em;
}

.green {
  color: #03be1c;
}

.btc {
  font-size: 0.8rem !important;
  word-break: break-all;
}

@media screen and (max-width: 520px) {
  .btc {
    font-size: 0.5rem;
  }
}

p.title {
    background-color: var(--bkg-color) !important;
    transition-duration: 0.3s;
}
</style>

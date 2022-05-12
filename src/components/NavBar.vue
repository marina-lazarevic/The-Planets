<template>
  <header class="header">
    <Transition name="header__branding-transition">
      <a href="/" v-show="show === true || this.screen_sm === false" class="header__branding"><span>The Planets</span></a>
    </Transition>
    <Transition name="header__nav-transition">
      <nav class="header__nav" v-show="nav_visibility === true || screen_sm === false">
        <button
            v-for="planet in planets"
            :key="planet.name"
            @click="$emit('getPlanet', planet)"
            :class="'header__nav-btn header__nav-btn--' + planetClassName(planet.name)">
            {{ planet.name }}
        </button>
      </nav>
    </Transition>
    <button class="header__toggle-btn" :class="{ close: this.nav_visibility }" @click="this.nav_visibility = !this.nav_visibility"
    v-if="screen_sm === true">
        <span class="header__bar"></span>
        <span class="header__bar"></span>
        <span class="header__bar"></span>
    </button>
  </header>
</template>
<script>
export default {
  name: "NavBar",
  props: ["planets", "planet"],
  emits: ["getPlanet"],
  data(){
      return {
        nav_visibility: false,
        screen_sm: true,
        show: false,
      }
  },
  methods: {
    screenSize() {
      window.innerWidth >= 768
        ? (this.screen_sm = false)
        : (this.screen_sm = true);
    },

    planetClassName(name) {
      let className = "";
      this.planet.name === name ? (className = "active") : (className = "");
      return name.toLowerCase() + " " + className;
    },
  },

  mounted(){
    this.show = true;
    this.screenSize();
    window.addEventListener("resize", () => {
      this.screenSize();
    });
  }
};
</script>
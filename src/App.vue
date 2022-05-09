<template>
  <header class="header">
    <a href="/" class="header__branding">The Planets</a>
    <nav class="header__nav" v-if="nav_visibility === true || screen_sm === false">
      <button v-for="planet in planets" :key="planet.name" @click="selectThePlanet(planet)">
        {{ planet.name }}
      </button>
    </nav>
    <button
      class="header__hamburger-btn"
      @click="this.nav_visibility = !this.nav_visibility"
      v-if="screen_sm === true"
    >
      <span class="header__bar"></span>
      <span class="header__bar"></span>
      <span class="header__bar"></span>
    </button>
  </header>
</template>

<script>
import json from "@/data/data.json";

export default {
  name: "App",
  data() {
    return {
      planets: json,
      planet: {},
      nav_visibility: false,
      screen_sm: true,
    };
  },

  methods: {
    selectThePlanet(planet) {
      this.planet = planet;
      console.log(this.planet);
    },

    screenSize() {
      window.innerWidth >= 768
        ? (this.screen_sm = false)
        : (this.screen_sm = true);
    },
  },

  mounted() {
    this.screenSize();
    window.addEventListener("resize", () => {
      this.screenSize();
    });
  },
};
</script>

<style lang="scss">
@use "@/scss/abstract/index" as a;
@use "@/scss/boilerplate" as b;

.header {
  @include a.d-flex(center, space-between);
  padding: 1rem 1.5rem 1.3rem;
  border-bottom: 1px solid rgba(a.$light, $alpha: 0.2);
  position: fixed;
  width: 100vw;
  top: 0;
  left: 0;

  &__branding {
    @include a.antonio-md;
  }

  &__hamburger-btn {
    width: 28px;
    height: 22px;
    position: relative;
  }

  &__bar {
    position: absolute;
    background-color: a.$light;
    height: 3px;
    width: 100%;
    left: 0;

    &:nth-of-type(1) {
      top: 0;
    }

    &:nth-of-type(2) {
      top: calc(50% - 2px);
    }

    &:nth-of-type(3) {
      bottom: 1px;
    }
  }
}
</style>

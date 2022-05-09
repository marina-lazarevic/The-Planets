<template>
  <header class="header">
    <a href="/" class="header__branding">The Planets</a>
    <Transition name="header__nav-transition">
    <nav class="header__nav" v-show="nav_visibility === true || screen_sm === false">
      <button v-for="planet in planets" :key="planet.name" @click="selectThePlanet(planet); this.nav_visibility = !this.nav_visibility;" :class="'header__planet-btn header__planet-btn--' + planet.name.toLowerCase()">
        <span>{{ planet.name }}</span>
      </button>
    </nav>
    </Transition>
    <button
      class="header__hamburger-btn"
      @click="this.nav_visibility = !this.nav_visibility"
      v-if="screen_sm === true">
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
  background-color: a.$dark;
  z-index: 10;

  &__branding {
    @include a.antonio-md;
    white-space: nowrap;
  }

  &__hamburger-btn {
    width: 28px;
    height: 22px;
    position: relative;
    transition: .5s a.$default-transition;

    &.close {
      transform: rotate(90deg);
      .header__bar {
        &:nth-of-type(1){
          transform: rotate(45deg) translate(7px, 6px);
        }

        &:nth-of-type(2){
          transform: rotate(-45deg);
        }

        &:nth-of-type(3){
          opacity: 0;
        }
      }
    }
  }

  &__bar {
    position: absolute;
    background-color: a.$light;
    height: 3px;
    width: 100%;
    left: 0;
    transition: transform .3s a.$default-transition;

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

  &__nav {
    position: absolute;
    height: calc(100vh - 74.07px);
    width: 100vw;
    top: calc(100% + 1.2px);
    left: 0;
    padding: 1.5rem;
    @include a.d-flex(flex-start, flex-start);
    flex-direction: column;
    background-color: a.$dark;
    transition: opacity 1.2s ease-out;
  }

  &__planet-btn {
    @include a.jost-bold-md;
    @include a.d-flex(center, unset);
    padding: 1.5rem 0;
    width: 100%;
    height: fit-content;
    overflow: hidden;

    &--mercury::before {
      background-color: a.$mercury;
    }
    &--venus::before {
      background-color: a.$venus;
    }
    &--earth::before {
      background-color: a.$earth;
    }
    &--mars::before {
      background-color: a.$mars;
    }
    &--jupiter::before {
      background-color: a.$jupiter;
    }
    &--saturn::before {
      background-color: a.$saturn;
    }
    &--uranus::before {
      background-color: a.$uranus;
    }
    &--neptune::before {
      background-color: a.$neptune;
    }

    &::before {
      content: "";
      width: 1.2em;
      height: 1.2em;
      border-radius: 50%;
      margin-right: 2em;
      flex-shrink: 0;
    }

    span, &::before {
      display: block;
      transition: 1.5s a.$default-transition;
    }

    $i: 1;

    @for $i from 1 through 8 {
      &:nth-of-type(#{$i}){
        span, &::before {
          transition-delay: #{$i * .07}s;
        }
      }
    }
  }

  &__nav-transition-enter-active,
  &__nav-transition-leave-active {
    opacity: 1;
    .header__planet-btn {
      span, &::before {
        transform: translateY(0);
      }
    }
  }

  &__nav-transition-enter-from,
  &__nav-transition-leave-to {
    opacity: 0;
    .header__planet-btn {
      span, &::before {
        transform: translateY(300%);
      }
    }
  }
}
</style>

<template>
  <header class="header" :style= "[this.nav_visibility == true && this.screen_sm == true ? {'position': 'fixed'} : {'position': 'relative'}]">
    <Transition name="header__branding-transition">
      <a href="/" v-show="show === true || this.screen_sm === false" class="header__branding">
        <span>The Planets</span>
      </a>
    </Transition>
    <Transition name="header__nav-transition">
      <nav class="header__nav" v-show="nav_visibility === true || screen_sm === false">
        <button
          v-for="planet in planets"
          :key="planet.name"
          @click="
            $emit('getPlanet', planet);
            this.nav_visibility = !this.nav_visibility;"
          :class="'header__nav-btn header__nav-btn--' + planetClassName(planet.name)">
          <span>{{ planet.name }}</span>
        </button>
      </nav>
    </Transition>
    <button
      class="header__toggle-btn"
      :class="{ close: this.nav_visibility }"
      @click="this.nav_visibility = !this.nav_visibility"
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
  data() {
    return {
      nav_visibility: false,
      screen_sm: true,
      show: false,
    };
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

  mounted() {
    this.show = true;
    this.screenSize();
    window.addEventListener("resize", () => {
      this.screenSize();
    });
  },
};
</script>

<style lang="scss">
@use "@/scss/abstract/index" as a;

.header {
  padding: 1rem 1.5rem 1.3rem;
  border-bottom: 1px solid rgba(a.$light, $alpha: 0.2);
  background-color: a.$dark;
  @include a.d-flex(center, space-between);
  z-index: 5;
  top: 0;
  left: 0;
  width: 100vw;

  @media screen and (min-width: a.$screen-md) and (max-width: a.$screen-lg) {
    flex-direction: column;
  }

  @media screen and (min-width: a.$screen-md) {
    position: static;
    background: none;
  }

  @media screen and (min-width: a.$screen-lg) {
    padding: 0 4vw;
  }

  &__branding {
    @include a.antonio-md;
    white-space: nowrap;
    height: fit-content;
    width: fit-content;
    overflow: hidden;

    span {
      display: block;
      width: fit-content;
      transition: 2s a.$default-transition;
    }

    @media screen and (min-width: a.$screen-md) and (max-width: a.$screen-lg) {
      padding: 1.3rem 0;
    }
  }

  &__branding-transition-enter-active,
  &__branding-transition-leave-active {
    @media screen and (min-width: a.$screen-md) {
      span {
        opacity: 1;
        transform: translateY(0);
      }
    }
  }
  &__branding-transition-enter-from,
  &__branding-transition-leave-to {
    @media screen and (min-width: a.$screen-md) {
      span {
        opacity: 0;
        transform: translateY(200%);
      }
    }
  }

  &__nav {
    position: absolute;
    top: calc(100% + 1px);
    left: 0;
    width: 100vw;
    height: calc(100vh - 74.9px);
    background-color: a.$dark;
    padding: 1rem 1.5rem 1.3rem;
    @include a.d-flex(flex-start, flex-start);
    flex-direction: column;
    

    @media screen and (min-width: a.$screen-md) {
      position: static;
      flex-direction: row;
      height: fit-content;
      width: fit-content;
      max-width: 100vw;
      padding: 0;
    }
  }

  &__nav-btn {
    @include a.jost-bold-md;
    @include a.d-flex(center, flex-start);
    padding: 1.2rem 0;
    height: fit-content;
    width: fit-content;
    overflow: hidden;
    transition: color 0.2s ease-out;

    &::before {
      content: "";
      width: 1.2em;
      height: 1.2em;
      border-radius: 50%;
      margin-right: 1.2rem;
    }

    &::before,
    span {
      display: block;
      transition: transform .8s a.$default-transition;
    }

    $i: 1;

    @for $i from 1 through 8 {
      &:nth-of-type(#{$i}) {
        &::before,
        span {
          transition-delay: #{$i * 0.1}s;
        }
      }
    }

    @media screen and (min-width: a.$screen-md) and (max-width: a.$screen-lg) {
      &::before {
        display: none;
      }
    }

    @media screen and (min-width: a.$screen-md) {
      @include a.jost-bold-sm;
      padding: 1.3rem 1.3rem 0;
      margin-bottom: 1.3rem;

      &:hover,
      &.active {
        color: a.$light;
      }
    }

    @media screen and (min-width: a.$screen-lg) {
      position: relative;
      padding: 1.7rem 1.3rem 0;
      margin-bottom: 1.7rem;

      &::before {
        position: absolute;
        top: 0;
        left: 0;
        width: 0;
        height: 3px;
        border-radius: 0;
        opacity: 0;
        transform: none !important;
      }

      &:hover,
      &.active {
        color: a.$light;
        &::before {
          opacity: 1;
          width: 100%;
        }
      }
    }
  }

  &__nav-transition-enter-from {
    .header__nav-btn {
      span,
      &::before {
        transform: translateY(300%);
      }
    }
  }

  &__toggle-btn {
    width: 28px;
    height: 22px;
    position: relative;
    transition: 1.2s a.$default-transition;
    &.close {
      transform: rotate(90deg);
      .header__bar {
        &:nth-of-type(1) {
          transform: rotate(45deg) translate(7px, 6px);
        }
        &:nth-of-type(2) {
          transition: 0s;
          width: 0;
        }
        &:nth-of-type(3) {
          transform: rotate(-45deg) translate(6px, -6px);
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
    transition: transform 1.2s a.$default-transition,
      width 1.2s a.$default-transition;
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
<template>
<header class="header">
  <Transition name="header__branding-transition">
    <a
      v-show="show === true || this.screen_sm === false"
      href="/"
      class="header__branding">
      <span>The Planets</span>  
    </a>
  </Transition>
  <Transition name="header__nav-transition">
    <nav
      class="header__nav"
      v-show="nav_visibility === true || screen_sm === false">
      <NavItem
        v-for="planet in planets"
        :key="planet.name"
        :name="planet.name"
        @click="$emit('getPlanet', planet)"
        :class="'header__planet-btn header__planet-btn--' + planetClass(planet.name)"/>
    </nav>
  </Transition>
  <button
    class="header__hamburger-btn"
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
import NavItem from "@/components/NavItem.vue";

export default {
  name: "MainNav",
  props: ["planets", "planet"],
  emits: ["getPlanet"],
  components: {
    NavItem,
  },

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

    planetClass(name) {
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

<style lang="scss">
@use "@/scss/abstract/index" as a;
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
  @media screen and (min-width: a.$screen-md) and (max-width: a.$screen-lg) {
    flex-direction: column;
    padding: 2rem 1.5rem;
  }
  @media screen and (min-width: a.$screen-md) {
    position: static;
  }
  @media screen and (min-width: a.$screen-lg) {
    padding: 0 1.5rem 0 2.3rem;
  }
  &__branding {
    @include a.antonio-md;
    white-space: nowrap;
    height: fit-content;
    width: fit-content;
    overflow: hidden;
    flex-shrink: 0;
    span {
      display: block;
      width: fit-content;
      transition: 2s a.$default-transition;
    }
    @media screen and (min-width: a.$screen-md) and (max-width: a.$screen-lg) {
      padding-bottom: 2rem;
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
  &__hamburger-btn {
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
    transition: opacity 1.5s ease-out;
    @media screen and (min-width: a.$screen-md) and (max-width: a.$screen-lg) {
      justify-content: center;
    }
    @media screen and (min-width: a.$screen-md) {
      flex-direction: row;
      position: static;
      height: fit-content;
      padding: 0;
    }
    @media screen and (min-width: a.$screen-lg) {
      justify-content: flex-end;
    }
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
    span,
    &::before {
      display: block;
      transition: transform 1.5s a.$default-transition;
    }
    $i: 1;
    @for $i from 1 through 8 {
      &:nth-of-type(#{$i}) {
        span,
        &::before {
          transition-delay: #{$i * 0.07}s;
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
      width: fit-content;
      padding: 0 0.8rem;
    }
    @media screen and (min-width: a.$screen-lg) {
      position: relative;
      transition: color 0.2s ease-out;
      padding-top: 1.8rem;
      margin-bottom: 1.8rem;
      &.active,
      &:hover {
        color: a.$light;
        &::before {
          opacity: 1;
        }
      }
      &::before {
        position: absolute;
        border-radius: 0;
        height: 3px;
        width: 100%;
        left: 0;
        top: 0;
        transform: none !important;
        opacity: 0;
      }
    }
  }
  &__nav-transition-enter-active,
  &__nav-transition-leave-active {
    opacity: 1;
    .header__planet-btn {
      span,
      &::before {
        transform: translateY(0);
      }
    }
  }
  &__nav-transition-enter-from,
  &__nav-transition-leave-to {
    opacity: 0;
    .header__planet-btn {
      span,
      &::before {
        transform: translateY(300%);
      }
    }
  }
}
</style>
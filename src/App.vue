<template>

<NavBar :planets="planets" :planet="planet" @getPlanet="showPlanet"/>
<PlanetPage :planet="planet"/>
<img src="@/assets/background-stars.svg" alt="stars" class="bg-img">
</template>

<script>
import json from '@/data/data.json';
import NavBar from '@/components/NavBar.vue';
import PlanetPage from '@/components/PlanetPage.vue';

export default {
  name: 'App',
  components: {
    NavBar,
    PlanetPage
  },
  data(){
    return {
      planets: json,
      planet: {}
    }
  },

  methods: {
    showPlanet(planet){
      this.planet = planet;
    }
  },

  created(){
    this.planet = this.planets[0]
  }
}
</script>

<style lang="scss">
@use '@/scss/abstract/index' as a;
@use '@/scss/boilerplate';

main {
  position: relative;
}

.bg-img {
  position: absolute;
  top: 0;
  left: 0;
  height: 100vh;
  z-index: -1;

  @media screen and (min-width: a.$screen-lg) {
    width: 100%;
    height: auto;
  }
}

.header__nav-btn,
.planet__toggle-btn {
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
}

</style>

<template>
<NavBar :planets="planets" :planet="planet" @getPlanet="showPlanet"/>
<PlanetPage :planet="planet"/>
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

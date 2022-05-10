<template>
  <MainNav :planets="planets" :planet="planet" @getPlanet="showPlanet"/>
  <ThePlanet :planet="planet"/>
</template>

<script>
import json from "@/data/data.json";
import MainNav from "@/components/MainNav.vue";
import ThePlanet from "@/components/ThePlanet.vue";

export default {
  name: "App",
  components: {
    MainNav,
    ThePlanet
  },

  data() {
    return {
      planets: json,
      planet: {},
    };
  },

  methods: {
    showPlanet(planet){
      this.planet = planet;
      console.log(planet);
    }
  },

  created() {
    this.planet = this.planets[0];
  },
};
</script>

<style lang="scss">
@use "@/scss/abstract/index" as a;
@use "@/scss/boilerplate" as b;
</style>

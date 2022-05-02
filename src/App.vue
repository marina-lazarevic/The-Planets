<template>

</template>

<script>
import json from '@/data/data.json';

export default {
  name: 'App',
  data() {
    return {
      planets: json,
    }
  }
}
</script>

<style lang="scss">
@use '@/scss/abstract' as a;

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background-color: a.$dark;
}
</style>

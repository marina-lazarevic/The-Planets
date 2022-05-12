<template>
<section class="planet">
  <div class="planet__row">
    <div class="planet__img-container">
        <img :src="require(`@/assets/planet-${planet.name.toLowerCase()}.svg`)" :alt="`${planet.name} planet`" v-show="content_type == 'geology' || content_type == 'overview'">
        <img :src="require(`@/assets/planet-${planet.name.toLowerCase()}-internal.svg`)" :alt="`${planet.name} structure`" v-show="content_type == 'structure'">
        <img :src="require(`@/assets/geology-${planet.name.toLowerCase()}.png`)" :alt="`${planet.name} geology`" v-show="content_type == 'geology'">
    </div>
    <div class="planet__info">
      <article class="planet__article">
        <h1 class="planet__name">{{ planet.name }}</h1>
        <p class="planet__content">{{article.content}}</p>
        <p class="planet__source">Source: <a :href="article.source">Wikipedia</a></p>
      </article>
      <div class="planet__btn-container">
        <button v-for="(item, index) in content" :key="index" @click="toggleContent(item)" :class='"planet__toggle-btn planet__toggle-btn--" + getKeyName(item) + " " + btnClassName(item)'>{{getKeyName(item)}}</button>
      </div>
    </div>
    <div class="planet__stats">
      <div v-for="(item, index) in stats" :key="index" class="planet__stats-container">
        <p class="planet__stats-type">{{getKeyName(item)}}</p>
        <h3 class="planet__stats-item">{{item}}</h3>
      </div>
    </div>
  </div>
</section>
</template>

<script>
export default {
  name: "PlanetPage",
  props: ["planet"],

  computed: {
  stats: function () {
    return [this.planet.rotation, this.planet.revolution, this.planet.radius, this.planet.temperature]
  },

  content: function () {
    this.article = this.planet.overview;
    return [this.planet.overview, this.planet.structure, this.planet.geology]
  }
},

  data() {
    return {
      article: {},
      content_type: 'overview'
    };
  },

  methods: {
    getKeyName(type){
      return Object.keys(this.planet).find(key => this.planet[key] === type);
    },

    toggleContent(content){
      this.article = content;
      this.content_type = this.getKeyName(content);
    },

    btnClassName(item) {
      let c = this.getKeyName(item);
      let className = '';
      c == this.content_type ? className = 'active' : className = '';
      return className; 
    },
  }
};
</script>
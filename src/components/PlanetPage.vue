<template>
  <section class="planet">
    <div class="planet__row">
      <div class="planet__img-container">
        <img
          :src="require(`@/assets/planet-${planet.name.toLowerCase()}.svg`)"
          :alt="`${planet.name} planet`"
          v-show="content_type == 'geology' || content_type == 'overview'"
          class="planet__img"
        />
        <img
          :src="
            require(`@/assets/planet-${planet.name.toLowerCase()}-internal.svg`)
          "
          :alt="`${planet.name} structure`"
          v-show="content_type == 'structure'"
          class="planet__img"
        />
        <img
          :src="require(`@/assets/geology-${planet.name.toLowerCase()}.png`)"
          :alt="`${planet.name} geology`"
          v-show="content_type == 'geology'"
          class="planet__img"
        />
      </div>
      <div class="planet__info">
        <article class="planet__article">
          <h1 class="planet__name">{{ planet.name }}</h1>
          <p class="planet__content">{{ article.content }}</p>
          <p class="planet__source">
            Source:
            <a :href="article.source"
              >Wikipedia
              <svg
                class="planet__svg-arrow"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 278.56 278.56"
              >
                <path
                  d="M50.91,24H254.56V227.65M240.42,38.14,33.94,244.62"
                /></svg
            ></a>
          </p>
        </article>
        <div class="planet__btn-container">
          <button
            v-for="(item, index) in content"
            :key="index"
            @click="toggleContent(item)"
            :class="
              'planet__toggle-btn planet__toggle-btn--' +
              planet.name.toLowerCase() +
              ' ' +
              btnClassName(item)
            "
          >
          <span class="planet__btn-index">{{index}}</span>
            {{ getKeyName(item) }}
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" class="planet__chevron">
            <path d="M328 112L184 256l144 144" />
            </svg>
          </button>
        </div>
      </div>
    </div>
    <Transition name="planet__stats-transition">
      <div class="planet__stats" v-if="show">
        <div
          v-for="(item, index) in stats"
          :key="index"
          class="planet__stats-container"
        >
          <p class="planet__stats-type">{{ getKeyName(item) }}</p>
          <h3 class="planet__stats-item">{{ item }}</h3>
        </div>
      </div>
    </Transition>
  </section>
</template>

<script>
export default {
  name: "PlanetPage",
  props: ["planet"],

  computed: {
    stats: function () {
      return [
        this.planet.rotation,
        this.planet.revolution,
        this.planet.radius,
        this.planet.temperature,
      ];
    },

    content: function () {
      this.article = this.planet.overview;
      return [this.planet.overview, this.planet.structure, this.planet.geology];
    },
  },

  data() {
    return {
      article: {},
      content_type: "overview",
      show: false,
    };
  },

  methods: {
    getKeyName(type) {
      return Object.keys(this.planet).find((key) => this.planet[key] === type);
    },

    toggleContent(content) {
      this.article = content;
      this.content_type = this.getKeyName(content);
    },

    btnClassName(item) {
      let c = this.getKeyName(item);
      let className = "";
      c == this.content_type ? (className = "active") : (className = "");
      return className;
    },
  },

  mounted() {
    this.show = true;
  },
};
</script>

<style lang="scss">
@use "@/scss/abstract/index" as a;

.planet {
  padding: 0 1.5rem 3rem;
  position: relative;

  @media screen and (min-width: a.$screen-lg) {
    padding: 0 6vw;
  }

  &__row {
    @media screen and (min-width: a.$screen-lg) {
      @include a.d-flex(center, space-between);
    }
  }

  &__img-container {
    width: 100%;
    height: fit-content;
    min-height: 450px;
    padding-top: 54.5px;
    @include a.d-flex(center, center);
    position: relative;

    img {
      transform: scale(0.4);
      position: absolute;
    }

    img:nth-of-type(3) {
      max-width: 100px;
      position: absolute;
      transform: translateY(80%);
    }

    @media screen and (min-width: a.$screen-md) {
      padding-top: 0;
      min-height: 500px;

      img {
        transform: scale(0.55);
      }
    }

    @media screen and (min-width: a.$screen-lg) {
      width: 100%;
      min-height: 85vh;
      img {
        transform: scale(0.8);
      }
    }
  }

  &__info {
    @media screen and (min-width: a.$screen-md) and (max-width: a.$screen-lg) {
      @include a.d-flex(center, space-between);
    }

    @media screen and (min-width: a.$screen-lg) {
      width: 35%;
      flex-shrink: 0;
    }
  }

  &__article {
    text-align: center;
    @media screen and (min-width: a.$screen-md) and (max-width: a.$screen-lg) {
      width: 45%;
    }
    @media screen and (min-width: a.$screen-md) {
      text-align: left;
      flex-shrink: 0;
    }
  }

  &__name {
    @include a.antonio-lg;
    margin-bottom: 2rem;
  }

  &__content {
    @include a.jost-regular-md;
    margin-bottom: 2rem;
  }

  &__source {
    @include a.jost-regular-md;
    margin-bottom: 2rem;

    a {
      color: rgba(a.$light, $alpha: 0.5);
      text-decoration: underline;
      font-weight: 700;
      transition: color 0.2s ease-out;

      &:hover {
        color: a.$light;

        svg > path {
            stroke: a.$light;
        }
      }
    }
  }

  &__svg-arrow {
    height: 0.6rem;
    width: auto;
    path {
      fill: none;
      stroke: #828290;
      stroke-linecap: square;
      stroke-width: 65px;
      transition: stroke 0.2s ease-out;
    }
  }

  &__btn-container {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    padding: 0 1.5rem;
    @include a.d-flex(center, space-between);
    border-bottom: 1px solid rgba(a.$light, $alpha: 0.2);

    @media screen and (min-width: a.$screen-md) and (max-width: a.$screen-lg) {
      width: 45%;
    }

    @media screen and (min-width: a.$screen-md) {
      position: static;
      border-bottom: none;
      flex-direction: column;
      align-items: flex-start;
      padding: 0;
      flex-shrink: 0;
    }
  }

  &__toggle-btn {
    @include a.jost-bold-sm;
    padding: 1.2rem 0;
    position: relative;
    transition: color 0.2s ease-out;

    &::before {
      content: "";
      position: absolute;
      left: 0;
      bottom: 0;
      height: 3px;
      width: 100%;
      opacity: 0;
    }

    &.active,
    &:hover {
      color: a.$light;
      &::before {
        opacity: 1;
      }
    }

    @media screen and (min-width: a.$screen-md) {
      padding: 1.2rem;
      width: 100%;
      text-align: left;
      margin-bottom: 1rem;
      border: 1px solid rgba(a.$light, $alpha: 0.2);
      color: a.$light;

      &:hover {
        border: 1px solid rgba(a.$light, $alpha: 0);
        background-color: a.$gray;
        transition: background-color .2s ease-out;
      }

      &::before {
        height: 100%;
        z-index: -1;
      }
    }

    @media screen and (min-width: a.$screen-lg) {
      &:hover {
        &::before {
          opacity: 0;
          transition: opacity .2s ease-out;
        }
      }
    }
  }

  &__btn-index {
      display: none;
      @media screen and (min-width: a.$screen-md) {
        display: inline-block;
        opacity: .5;
        margin-right: 1.2rem;
        &::before {
            content: '0';
        }
      }
  }

  &__chevron {
    display: none;
    @media screen and (min-width: a.$screen-md) {
        display: block;
        height: 1.5em;
        width: auto;
        position: absolute;
        top: 50%;
        right: 1.2rem;
        transform: translateY(-50%) rotate(180deg);
        path {
            fill: none;
            stroke: rgba(a.$light, $alpha: .5);
            stroke-linecap: square;
            stroke-miterlimit: 10;
            stroke-width: 65px;
        }
    }
  }

  &__stats {
    height: fit-content;
    width: 100%;
    overflow: hidden;
    @media screen and (min-width: a.$screen-md) {
      @include a.d-flex(center, space-between);
    }
  }

  &__stats-container {
    border: 1px solid rgba(a.$light, $alpha: 0.2);
    @include a.d-flex(center, space-between);
    padding: 1rem 1.2rem;
    margin-bottom: 1rem;
    transition: transform 1.2s a.$default-transition;

    @media screen and (min-width: a.$screen-md) {
      flex-direction: column;
      align-items: flex-start;
      width: 23.5%;
    }

    $i: 1;

    @for $i from 1 through 4 {
      &:nth-of-type(#{$i}) {
        transition-delay: #{$i * 0.1}s;
      }
    }
  }

  &__stats-type {
    @include a.jost-bold-sm;
    white-space: nowrap;
    @media screen and (min-width: a.$screen-md) {
      margin-bottom: 0.5rem;
    }
  }

  &__stats-item {
    @include a.antonio-md;
    white-space: nowrap;
  }

  &__stats-transition-enter-from {
    @media screen and (min-width: a.$screen-md) {
      .planet__stats-container {
        transform: translateY(200%);
      }
    }
  }
}
</style>
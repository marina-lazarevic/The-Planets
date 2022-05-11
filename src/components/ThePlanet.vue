<template>
    <section class="planet">
        <div class="planet__row">
            <div class="planet__img-container"></div>
            <article class="planet__article">
                <div class="planet__article-info">
                    <h1 class="planet__name">{{planet.name}}</h1>
                    <PlanetContent :content="content"/>
                </div>
                <div class="planet__controls">
                    <ContentToggleButton v-for="item in content_type" :key="item" :c_type="getType(item)" @click="getContent(item)" class="planet__control-btn"/>
                </div>
            </article>
        </div>
    </section>
</template>

<script>
import ContentToggleButton from '@/components/ContentToggleButton.vue';
import PlanetContent from '@/components/PlanetContent.vue';

export default {
    name: 'ThePlanet',
    props: ['planet'],
    components: {
        ContentToggleButton,
        PlanetContent
    },

    data(){
        return {
            content_type: [],
            content: this.planet.overview,
        }
    },

    methods: {
        getType(type){
            return Object.keys(this.planet).find(key => this.planet[key] === type);
        },

        getContent(content){
            this.content = content;
            console.log(this.content);
        }
    },

    beforeMount(){
        this.content_type.push(this.planet.overview, this.planet.structure, this.planet.geology)
    },
}
</script>
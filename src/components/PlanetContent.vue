<template>
    <p>{{content.content}}</p>
    <p>Source: <a :href='content.source'>Wikipedia</a></p>
</template>

<script>
export default {
    name: 'PlanetContent',
    props: ['content']
}
</script>
<template>
    <div>
        <p>{{d_type}}</p>
        <h3>{{d_item}}</h3>
    </div>
</template>

<script>
export default {
    name: 'PlanetData',
    props: ['d_type', 'd_item']
}
</script>
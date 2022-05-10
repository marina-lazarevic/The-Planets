<template>
    <button>
        <span>{{name}}</span>
    </button>
</template>

<script>
export default {
    name: 'NavItem',
    props: ['name']
}
</script>
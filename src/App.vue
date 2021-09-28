<template>
    <div id="app" class="container">
        <field />
    </div>
</template>

<script>
    import Field from './components/Field.vue'

    export default {
        name: 'App',
        components: {Field}
    }
</script>

<style lang="sass">
    body
        background: url("./assets/images/background.jpg") repeat scroll
</style>

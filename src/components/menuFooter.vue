<template>
    <footer :style="`background-color: ${footerColor}`"></footer>
</template>

<script>
    export default {
        name: 'footer',
        data(){
            return {
                footerColor: "slategrey"
            };
        },
        methods: {
        },
        computed: {
        },
        //mounted() {
        //},
        components: {
        },
    }
</script>

<style>

</style>

<script>
export default{
    methods: {
        gotoAbout() {
            this.$router.push('/about')
        },
    },
}
</script>

<template>
    <h2>HomeView</h2>
    <button @click="gotoAbout">Go To About</button>
</template>
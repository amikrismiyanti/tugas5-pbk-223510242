<script setup>
import { computed } from 'vue'
import { useRoute , useRouter } from 'vue-router'

const router = use()
const route = useRoute()

const search = computed({
    get() {
        return route.query.search ?? ''
    }
})
</script>

<template>
    <h2>AboutView</h2>
    <label>
        search: <input v-model.trim="search" maxlength="20">
    </label>
</template>

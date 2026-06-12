<script setup>
    import {ref} from 'vue'
    const title = ref('Quote Generator')

</script>

<template>
    <header>
        <h1>{{ title }}</h1>
    </header>
</template>

<style scoped>
    header {
        display: flex;  
        font-family:'Georgia', serif;
        color:#CCD6D9;
        margin-bottom:25px;
        justify-content: center;
        align-items: center;
        font-size: 30px;
    }
</style>
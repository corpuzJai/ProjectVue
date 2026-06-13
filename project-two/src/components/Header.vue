<script setup>
    import {ref} from 'vue'
    const title = ref("Janna's Anik-Aniks")

</script>

<template>
    <header class="header-container">
        <h1 class="title">
            {{ title }}
        </h1>
    </header>

</template>

<style scoped>
    .header-container {
        max-width: 90%;
        text-align: left;
        margin-top: 1.2rem;
    }

    .title {
        font-family:'Courier New', Courier, monospace;
        font-size: 2.2rem;
        margin: 0;        
    }

</style>
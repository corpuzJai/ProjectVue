<script setup>
    import {ref} from 'vue'
    const title = ref('Quote Generator')

</script>

<template>
    <header class="header-container">
        <h1 class="title">{{ title }}</h1>
    </header>
</template>

<style scoped>
    .header-container {
        max-width: 90%;
        width: 1100px;
        text-align: left;
        box-sizing: border-box;
        margin: 0 0 15px 0;
    }

    .title {
        font-family: "Book Antiqua", Palatino; 
        color:#26383f;
        font-size: 3.2rem;
        font-weight: 800;
        margin: 0;
    }
</style>
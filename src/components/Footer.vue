<script setup>
    import {ref} from 'vue'
    const year = ref('2026')
</script>

<template>
    <footer>
        {{ year }}
    </footer>
</template>

<style scoped>
footer {
    position:fixed;
    margin: 0 auto;
    bottom:0;
    text-align:center;
    color:#CCD6D9;
    padding-bottom:20px;
}

span{
    font-family: cursive;
}
</style>
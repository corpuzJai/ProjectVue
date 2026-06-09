<script setup>
import {ref, onMounted} from 'vue'
const quoteData = ref(null)
const isLoading = ref(true)

const api_url = "https://dummyjson.com/quotes/random"

const getapi = () => {
    fetch(api_url)
        .then(response => response.json())
        .then(data => {
            quoteData.value = data
            isLoading.value = false
        })
        .catch(error => {
            console.error("Error fetching quote:", error)
            isLoading.value = false
        })
}

onMounted(() => {
    getapi()
})

</script>

<template>
    <main class="main-content">
        <div class="card">
            <div v-if="isLoading" class="loading-state">
                Loading...
            </div>

            <div v-else class="quote-content">
                <p class="text">{{ quoteData.quote }}</p>
                <p class="author">- {{ quoteData.author }}</p>
            </div>

            <button @click="getapi" :disabled="isLoading" class="refresh-btn">
                Another!
            </button>
        </div>
    </main>
</template>

<style scoped>
.main-content {
    font-family: system-ui, sans-serif;
    background: #CCD6D9;
    max-width:90%;
    border-radius:0 15px 0 15px;
    padding:35px;
    display:flex;
    flex-direction:column;
    align-items:center;
    gap:10px;
}

main section {
    display: flex;
    width:100%;
    flex-direction:column;
    gap:10px;
    margin-bottom:25px;
}

main p {
    font-weight:bold;
    font-style: italic;
    font-size:2rem;
    text-align:left;
}

main p::before {
    content: '"'
}

main p::after {
    content: '"'
}

main span {
    align-self:end;
    color: #406473;
}

main span::before {
     content: "- "
}

a {
    text-decoration: none;
}

main button {
    background:#406473;
    color: white;
    padding:10px;
    border: 0;
    font-size:1.2rem;
    border-radius:0 5px 0 5px;
    font-weight:bold;
    margin-top:20px;
    cursor:pointer;
    transition: transform 0.2s;
}
main button:hover{
    transform: scale(1.05);
}   
</style>
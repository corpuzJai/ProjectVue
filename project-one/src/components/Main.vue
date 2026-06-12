<script setup>
import {ref, computed, onMounted} from 'vue'
const quoteData = ref(null)
const isLoading = ref(true)

const isModalOpen = ref(false)
const savedQuotesList = ref([])

const savedSearchQuery = ref('')

const filteredQuotes = computed(() => {
    if (!savedSearchQuery.value.trim()) {
        return savedQuotesList.value
    }

    const query = savedSearchQuery.value.toLowerCase().trim()

    return savedQuotesList.value.filter(quote =>
        quote.text.toLowerCase().includes(query) ||
        quote.author.toLowerCase().includes(query)
    )
})

const saveQuotes = () => {
    if (!quoteData.value) return

    const saved = {
        id: Date.now(),
        text: quoteData.value.quote,
        author: quoteData.value.author
    }

    let savedQuotes = JSON.parse(localStorage.getItem('savedQuotes')) || []
    const isAlreadySaved = savedQuotes.some(q => q.text === saved.text)
    if (isAlreadySaved) {
        alert('This quote is already saved!')
        return
    }
    savedQuotes.push(saved)
    localStorage.setItem('savedQuotes', JSON.stringify(savedQuotes))

    savedQuotesList.value = savedQuotes
}

const openSavedQuotes = () => {
    savedQuotesList.value = JSON.parse(localStorage.getItem('savedQuotes')) || []
    isModalOpen.value = true
}

defineExpose({ openSavedQuotes })

const deleteQuote = (index, quote) => {
    const confirmDelete = confirm(`Remove this quote from Saved Quotes?\n`)
    if (!confirmDelete) return
    savedQuotesList.value.splice(index, 1)
    localStorage.setItem('savedQuotes', JSON.stringify(savedQuotesList.value))
}

const closeModal = () => {
    isModalOpen.value = false
}

const api_url = "https://dummyjson.com/quotes/random"

const getapi = () => {
    isLoading.value = true
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

        </div>
    </main>
    
    <div class="button-container">
        <button @click="getapi" :disabled="isLoading" class="refresh-btn">
            Another!
        </button>
        <button @click="saveQuotes" class="save-btn"> Save </button>
    </div>

    <div v-if="isModalOpen" class="modal-overlay" @click.self="closeModal">
        <div class="modal-window">
            <h2>Saved Quotes</h2>

            <input 
                v-model="savedSearchQuery" 
                type="text"
                placeholder="Type keywords or author name"
                class="model-search-input"
                v-if="savedQuotesList.length > 0"
            />

            <p v-if="savedQuotesList.length === 0">No saved quotes yet.</p>

            <p v-else-if="filteredQuotes.length === 0" class="no-matches">
                No saved quotes match "{{ savedSearchQuery }}"
            </p>
            
            <div v-else class="quotes-scroll-card">
                <div v-for="(quote, index) in filteredQuotes" :key="index" class="saved-quote-card">
                    <div class="quote-wrapper">
                        "{{ quote.text }}" - {{ quote.author }}
                    </div>
                    <button @click="deleteQuote(index, quote.text)" class="row-delete-btn" title="Delete Quote">
                       🗑️ 
                    </button>
                </div>
            </div>
            <button @click="isModalOpen = false" class="close-btn">Close</button>
        </div>
    </div>
</template>

<style scoped>
.main-content {
    font-family: Candara, Calibri;
    background: #fefae0;
    max-width:90%;
    width: 1100px;
    height: 250px;
    border-radius:6px 6px 6px 6px;
    padding:30px;
    display:flex;
    flex-direction:column;
    gap:10px;
    box-sizing: border-box;
}

.card {
    display:flex;
    flex-direction: column;
    align-items:center;
    justify-content:center;
}

.text {
    font-weight:bold;
    font-style: italic;
    font-size:2rem;
    text-align:left;
    margin-bottom:10px;
}

.text::before {
    content: '"'
}

.text::after {
    content: '"'
}

.author {
    font-size:1.5rem;
    color:#26383f;
    align-self:end;
}

.button-container {
    display:flex;
    gap:15px;
    margin-top:5px;
}


.refresh-btn {
    background:#f2cc8f;
    color: rgb(0, 0, 0);
    padding:10px;
    border: 0;
    font-size:1.2rem;
    border-radius:0 5px 0 5px;
    font-weight:bold;
    margin-top:20px;
    cursor:pointer;
    transition: transform 0.2s;
}

.save-btn{
    background:#a44a3f;
    color: white;
    padding:10px;
    border: 0;
    font-size:1.2rem;
    border-radius:0 5px 0 5px;
    font-weight:bold;
    margin-top:20px;
    cursor:pointer;
    transition: transform 0.2s;
    align-self: flex-end;
}

.refresh-btn:hover, .save-btn:hover{
    transform: scale(1.05);
}   

.modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: auto;
}

.modal-search-input {
    width: 100%;
    padding: 10px 14px;
    border: 1px solid #b0c4de;
    font-size: 1rem;
    margin-bottom: 15px;
    outline: none;
    box-sizing: border-box;
}

.modal-search-input:focus {
    border-color: #406473;
    box-shadow: 0 0 5px rgba(64, 100, 115, 0.3)
}

.no-matches {
    color: #555;
    font-style: italic;
    margin: 20px 0;
    text-align: center;;
}

.modal-window {
    background: rgb(188, 221, 252);
    padding: 20px;
    border-radius: 8px;
    width: 80%;
    max-width: 800px;
    max-height: 50vh;
    display: flex;
    flex-direction: column;
    overflow-y: auto;
}

.modal-window h2 {
    margin-top: 0;
    margin-bottom: 15px;
}
.quotes-scroll-card {
    overflow-y: auto;
    margin: 15px 0;
    padding-right: 10px;
    flex-grow: 1;
}

.saved-quote-card {
    border-bottom: 1px solid #ccc;
    padding: 10px 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 15px;
}

.quote-wrapper {
    flex: 1;
    line-height: 1.4;
}

.row-delete-btn {
    background: none;
    border: none;
    font-size: 1.1rem;
    cursor: pointer;
    padding: 6px;
    border-radius: 4px;
    transition: background-color 0.2s, transform 0.1s;
}

.close-btn {
    background: #406473;
    color: white;
    padding: 10px;
    border: 0;
    font-size: 1rem;
    border-radius: 0 5px 0 5px;
    font-weight: bold;
    align-self: flex-end;
    cursor: pointer;
    transition: transform 0.2s;
}

</style>
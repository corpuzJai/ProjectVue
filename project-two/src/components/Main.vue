<script setup>
    import {ref, computed, onMounted} from 'vue'

    const productData = ref([])
    const isLoading = ref(true)
    const errorMessage = ref (null)

    const filterCategory = ref('')

    const USD_TO_PHP_RATE = 60.771

    const api_url ="https://dummyjson.com/products"

    const getapi = () => {
        isLoading.value = true
        errorMessage.value = null
        fetch(api_url)
            .then(response => {
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`)
                }
                return response.json()
            })
            .then(data => {
                productData.value = data.products
                isLoading.value = false
            })
            .catch(error => {
                console.error("Error fetching products:", error)
                errorMessage.value = error.message
                isLoading.value = false
            })
    }

    const uniqueCategories = computed(() => {
        const categories = productData.value.map(product => product.category)
        return Array.from(new Set(categories))
    })

    const filteredProducts = computed(() => {
        if(!filterCategory.value) {
            return productData.value    // If "All Categories", show everything
        }

        return productData.value.filter(product =>
            product.category === filterCategory.value
        )
    })


    onMounted(() => {
        getapi()
    })

</script>

<template>
    <div class="app-container">
        <h1>Tindahan</h1>

        <div class="filter-container">
            <label for="category-select" class="filter-label">Filter</label>
            <select
                id="category-select"
                v-model="filterCategory"
                class="category-dropdown"
            >
                <option value="">All Categories</option>
                <option
                    v-for="category in uniqueCategories"
                    :key="category"
                    :value="category"
                >
                    {{ category }}
                </option>
            </select>
        </div>

        <div v-if="isLoading" class="loading-state">
            Loading products...
        </div>

        <div v-else-if="errorMessage" class="status-msg error">
            Failed to load products: {{ errorMessage }}
        </div>

        <div v-else class="products-grid">
            <div v-for="(product, index) in filteredProducts" :key="product.id" class="products-card" :class="{ 'expand-up': index >= 8 }">
                <img :src="product.thumbnail" :alt="product.title" />
                <h3>{{ product.title }}</h3>
                <p class="category">{{ product.category }}</p>
                <p class="price">₱{{ (product.price *USD_TO_PHP_RATE).toFixed(2) }}</p>

                <div class="extra-details-hover">
                    <p class="description">{{ product.description }}</p>
                    <p class="rating"> {{ product.rating }}</p>
                    <p class="availability">
                        <span :class="product.availabilityStatus?.toLowerCase() === 'in stock' ? 'status-ok' : 'status-low'">
                            {{ product.availabilityStatus || 'In Stock' }}
                        </span>
                    </p>
                </div>
            </div>
        </div>
    </div>

</template>


<style scoped>
    .app-container {
        width: 100%;
        max-width: 1024px;
        margin: 0 auto;
        padding: 20px;
        box-sizing: border-box;
        font-family:Verdana, Geneva, Tahoma, sans-serif;
    }

    .filter-container {
        display: flex;
        align-items: left;
        align-self: center;
        gap: 8px;
        margin-bottom: 30px;
        width: 100%
    }

    .filter-label {
        font-size: 0.9rem;
        color: #555;
        font-weight: bold;
        align-self: center;
    }

    .category-dropdown {
        width: 100%;
        max-width: 320px;
        padding: 10px 14px;
        font-size: 1rem;
        border: 2px solid #ddd;
        border-radius: 8px;
        outline: none;
        background-color: white;
        cursor: pointer;
        text-transform: capitalize;
    }

    .category-dropdown:focus {
        border-color: #2c3e50;
    }

    .loading {
        text-align: center;
        font-size: 1.2rem;
        margin-top: 40px;
    }

    .status-msg {
        text-align: center;
    }

    .error {
        color: #d9534f;
    }

    .products-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
        gap: 20px;
        margin-top: 20px;
        align-items: start;
    }

    .products-card {
        position: relative;
        border: 1px solid #eee;
        border-radius: 8px;
        padding: 15px;
        text-align: center;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        align-items: center;
        background-color: white;
        box-sizing: border-box;
        transition: border-color 0.25s ease-in-out;
        z-index: 1;
    }

    .products-card:hover {
        transform: translateY(-4px);
        border-color: #38bdf8;
        background-color: #f8fafc;
        z-index: 10;
    }

    .products-card h3 {
        font-size: 1rem;
        margin: 12px 0 5px 0;
        color: #2c3e50;
    }

    .products-card img {
        width: 160px;
        height: 160px;
        object-fit: contain;
        background-color: #f9f9f9;
        border-radius: 4px;
        border-color: antiquewhite;
    }

    .category {
        color: #777;
        font-size: 0.9rem;
        text-transform: uppercase;
    }

    .price {
        font-weight: bold;
        color: #2c3e50;
    }

    .extra-details-hover {
        position: absolute;
        top: 100%;
        left: -1px;
        right: -1px;
        opacity: 0;
        visibility: hidden;
        text-align: left;
        font-size: 0.85rem;
        color: #334155;
        line-height: 1.4;
        box-sizing: border-box;
        transition: all 0.15s ease-in-out, visibility 0.15s ease-in-out;
        background-color: antiquewhite;
        border: 1px solid #38bdf8;
    }

    .products-card:not(expand-up):hover .extra-details-hover {
        max-height: 250px;
        opacity: 1;
        margin-top: 15px;
        padding-top: 15px;
        border-top: 1px dashed #cbd5e1;
        visibility: visible;
    }

    .products-card.expand-up:hover .extra-details-hover {
        top:auto;
        bottom: 100%;
        opacity: 1;
        visibility: visible;

        border-top: 1px solid #38bdf8;
        border-bottom: none;       
        border-top-left-radius: 8px;
        border-top-right-radius: 8px;
        border-bottom-left-radius: 0;
        border-bottom-right-radius: 0;
    }

    .products-card.expand-up:hover {
        transform: translateY(4px);
    }

    .extra-details-hover p {
        font-size: 0.85rem;
        text-align: center;
        color: #334155;
        line-height: 1.4;
    }

    .status-ok {
        color: #10b981;
        font-weight: bold;
        background-color: #ecfdf5;
        padding: 2px 6px;
        border-radius: 4px;
    }

    .status-low {
        color: #f59e0b;
        font-weight: bold;
        background-color: #fffbeb;
        padding: 2px 6px;
        border-radius: 4px;
    }

    @media (min-width: 1024px) {
        .products-grid {
            grid-template-columns: repeat(4, 1fr);
        }
    }

</style>
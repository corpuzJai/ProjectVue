<script setup>
    import {ref} from 'vue'
    
    defineProps({
        activeTab: {
        type: String,
        required: true
        }
    })
  
  const emit = defineEmits(['updateTab'])

  const changeTab = (tabName) => {
    emit('updateTab', tabName)
  }
    const title = ref("Janna's Anik-Aniks")

</script>

<template>
    <nav class="sidebar-nav">
        <div class="store-branding">
            <h1 class="title">
                {{ title }}
            </h1>
            <p class="tagline">Mga kung ano-ano, nandito!</p>
        </div>

        <ul class="nav-links">
            <li>
                <a href="#" class="{ active: activeTab === 'tindahan' }" @click.prevent="changeTab('tindahan')">
                    <span class="icon">🏠</span> Tindahan
                </a>
            </li>
            <li>
                <a href="#" class="{ active: activeTab === 'bilihin' }" @click.prevent="changeTab('bilihin')">
                    <span class="icon">🛒</span> Bilihin
                </a>
            </li>
            <li>
                <a href="#" class="{ active: activeTab === 'bayarin' }" @click.prevent="changeTab('bayarin')">
                    <span class="icon">📋</span> Bayarin
                </a>
            </li>
        </ul>

        <div class="store-sign">
            Bawal bumili ang hindi mag babayad!
        </div>
    </nav>

</template>

<style scoped>
    .sidebar-nav {
        width: 260px;
        min-width: 260px;
        background-color: #1e293b;
        color: #f8fafc;
        min-height: 100vh;
        padding: 30px 20px;
        box-sizing: border-box;
        display: flex;
        flex-direction: column;
    }

    .store-branding {
        text-align: center;
        margin-bottom: 20px;
        border-bottom: 1px solid #334155;
        padding-bottom: 10px;
    }

    .store-branding h2  {
        font-size: 1.5rem;
        margin: 0;
        font-family: Verdana, sans-serif;
        color: #38bdf8;
    }

    .store-tagline {
        font-size: 0.8rem;
        color: #94a3b8;
        margin: 5px 0 0 0;
        font-style: italic;
    }

    .nav-links {
        list-style: none;
        padding: 0;
        margin: 0;
        display: flex;
        flex-direction: column;
        gap: 12px;
    }
    
    .nav-links li a {
        display: flex;
        align-items: center;
        gap: 12px;
        color: #cbd5e1;
        text-decoration: none;
        padding: 12px 16px;
        border-radius: 8px;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        font-weight: 500;
        transition: all 0.2s ease;
    }
    
    .nav-links li a:hover {
        background-color: #334155;
        color: #fff;
    }
    
    .nav-links li a.active {
        background-color: #BEE1E6 !important;
        color: #0f172a !important;
        font-weight: bold;
        text-decoration: none;
    }
    
    .nav-links li.active a .icon {
        color: #0f172a !important;
    }
    
    .icon {
        font-size: 1.2rem;
    }

    .store-sign {
        margin-top: 15px;;
    }

</style>
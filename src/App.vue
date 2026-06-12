<script setup>
  import {ref} from 'vue'
  import Header from '@/components/Header.vue'
  import Main from '@/components/Main.vue'
  import Footer from '@/components/Footer.vue'

  const mainComponent = ref(null)

  const triggerSavedView = () => {
    console.log('Triggering saved quotes view...')
    if (mainComponent.value) {
      console.log('Calling openSavedQuotes on Main component')
      mainComponent.value.openSavedQuotes()
    } else {
      console.warn('Main component reference is not available')
    }
  }
</script>

<template>
  <div class="app-container">
    <nav class="navbar">
      <div class="nav-logo">💡</div>
      <div class="nav-links">
        <a href="#" class="nav-item">Home</a>
        <a href="#" class="nav-item" @click.prevent="triggerSavedView">Saved Quotes</a>
        
      </div>
    </nav>

    <Header />
    
    <Main ref="mainComponent" />

    <Footer />
  </div>

</template>

<style scoped>

.nav-links {
  display: flex;
  align-items: center;
  gap: 20px;
}

a {
  text-decoration: none;
  color: #CCD6D9;
}


</style>

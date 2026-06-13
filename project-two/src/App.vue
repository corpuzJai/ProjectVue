<script setup>
  import { ref } from 'vue';
  import Header from '@/components/Header.vue'
  import Main from '@/components/Main.vue'

  //const mainComponent = ref(null)

  
</script>

<template>
  <div >
    <Header />
    <Main />
  </div>
</template>

<style scoped>
  .layout-center-wrapper {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
    box-sizing: border-box;
  }

@media (min-width: 1024px) {
  .logo {
    margin: 0 2rem 0 0;
  }

}
</style>

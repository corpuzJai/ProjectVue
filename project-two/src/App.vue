<script setup>
  import { ref } from 'vue';
  import Header from '@/components/Header.vue'
  import Main from '@/components/Main.vue'

  const currentTab = ref('tindahan')

  const changeTab = (tabName) => {
    currentTab.value = tabName
  }
  
</script>

<template>
  <div class="layout-side-wrapper">
    <Header :activeTab="currentTab" @updateTab="changeTab"/>

    <div class="content-body">
      <div v-if="currentTab === 'tindahan'">
        <Main />
      </div>

      <div v-else-if="currentTab === 'bilihin'" class="placeholder-view">
        <h2> Mga Bilihin </h2>
        <p> Dito makikita ang mga pinamili.</p>
        <div class="empty-state">Wala pa.</div>
      </div>

      <div v-else-if="currentTab === 'bayarin'" class="placeholder-view">
        <h2> Mga Bayarin </h2>
        <p> Mga kailangan barayan sa tindahan.</p>
        <div class="empty-state"> Congrats! Wala kang utang.</div>
      </div>

    </div>
  </div>

</template>

<style scoped>
  .layout-side-wrapper {
    display: flex;
    flex-direction: row;
    min-height: 100vh;
    width: 100%;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .content-body {
    flex-grow: 1;
    min-width: 1024px;
    max-width: 1024px;
    width: 1024px;
    padding: 20px;
    box-sizing: border-box;
  }

  .placeholder-view p {
    color: #64748b;
    font-size: 0.95rem;
    margin-bottom: 20px;
  }

  .empty-state {
    text-align: center;
    padding: 40px;
    background-color: #f8fafc;
    border: 2px dashed #cbd5e1;
    border-radius: 8px;
    color: #94a3b8;
    font-style: italic;
  }

  @media (min-width: 1024px) {
    .logo {
      margin: 0 2rem 0 0;
    }

  }
</style>

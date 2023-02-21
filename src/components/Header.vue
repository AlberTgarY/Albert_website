<template>
    <div class="text-center my-3">
      <header> 
      <h1>Albert Website</h1>
      </header>
      <div class="d-flex justify-content-center">
        <HeaderLink to="/" exact>Home</HeaderLink>
        <HeaderLink to="/repo">Repo Search</HeaderLink>
      </div>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue'
  import HeaderLink from './HeaderLink.vue'
  
  export default defineComponent({
    components: { HeaderLink }
  })
  </script>
  
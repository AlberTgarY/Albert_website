<template>
   

      <header> 
      <h1>Albert Website</h1>
      </header>
      <a class="btn btn-primary" data-bs-toggle="offcanvas" href="#offcanvasExample" role="button" aria-controls="offcanvasExample">
      Link with href
      </a>

      <div class="offcanvas offcanvas-start" tabindex="-1" id="offcanvasExample" aria-labelledby="offcanvasExampleLabel">
        <div class="offcanvas-header">
          <h5 class="offcanvas-title" id="offcanvasExampleLabel">Menu</h5>
          <button type="button" class="btn-close text-reset" data-bs-dismiss="offcanvas" aria-label="Close"></button>
        </div>
        <div class="offcanvas-body">
          <div class="btn-group-vertical me-2" role="group">
                <HeaderLink to="/" exact>Home</HeaderLink>
                <HeaderLink to="/repo">Repo Search</HeaderLink>

            </div>
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

<style scoped>

</style>
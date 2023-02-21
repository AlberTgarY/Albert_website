
<template>
  <!-- <Header />
  <div class="container my-3">
    <router-view />
  </div> -->
  <div class="container text-center">
  <div class="row justify-content-md-center">
    <div class="col-md-auto">
      <Header />
    </div>
    <div class="col-md-auto">
      <router-view />
    </div>
  </div>
</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import Header from './components/Header.vue'
export default defineComponent({
  components: { Header }
})
</script>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>

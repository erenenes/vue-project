<script >
import Paginator from '../components/Paginator.vue';

export default {
  name: 'App',
  components: { Paginator },

  data() {
    return {
      currentPage:5,
      totalPage:20,
      edges:2,
      closeToCurrentPage:1
    }
  }
}
</script>


<template>
  <div class="about">
    <Paginator 
    :currentPage="currentPage"
    :totalPage="totalPage"
    :edges="edges"
    :closeToCurrentPage="closeToCurrentPage"  />
    
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
}
</style>

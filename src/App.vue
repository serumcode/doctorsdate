<template>
  <v-app>
    <navigation />

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>
import Navigation from '@/partials/Navigation';
export default {
  name: 'App',

  data: () => ({
    //
  }),
  components:{
    Navigation
  }
};
</script>
<style lang="scss">
:root{
  --secondary--light:#c3e4ea;
  --primary:#f98821;
  --secondary--dark:#177582;
  --secondary:#6fafb7;
}
#app{
  background: #f7f7f7;
  .primary{
    background-color:var(--primary) !important;
  }
  .secondary--text{
    color: var(--secondary) !important;
  }
  .primary--text{
    color:var(--primary) !important
  }
}
</style>

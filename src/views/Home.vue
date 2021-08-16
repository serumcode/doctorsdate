<template>
  <v-container class="logo-cont">
    <div class="d-flex justify-center">
    <v-img :src="require('@/assets/doctor.svg')" max-width="500"/>
    
    </div>
    <div class="logo-title text-center primary--text">Doctor's <span class="secondary--text">Date</span></div>
  </v-container>
</template>

<script>
  

  export default {
    name: 'Home',

    components: {
      
    },
  }
</script>
<style lang="scss">
.logo-cont{
  position: relative;
  top:50%;
  transform: translateY(-50%);
.logo-title{
      font-size: 80px;
}
}
</style>
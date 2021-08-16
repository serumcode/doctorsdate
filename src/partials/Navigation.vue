<template>
    
<v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
           <v-btn icon
        to="/"
      >
        <v-icon>fas fa-home</v-icon>
      </v-btn>
         <v-btn
        to="dates" text
      >
        <span class="mr-2">Calendar</span>
      </v-btn>
      </div>

      <v-spacer></v-spacer>

      
    </v-app-bar>
    </template>

    <script>

  export default {
    name: 'Navigation',

    components: {
    },
  }
</script>
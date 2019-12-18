<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-app-title>
          {{ title }}
        </v-app-title>
      </div>

      <v-spacer></v-spacer>
      <v-btn  color="white" dark v-on="on" icon>
          <editModal v-bind:icon="icon"/>
        </v-btn>
    </v-app-bar>

    <v-content>
      <home/>
    </v-content>
    
  </v-app>
</template>

<script>
import Home from './views/Home';
import EditModal from '@/views/EditModal'
export default {
  name: 'App',

  components: {
    Home,
    EditModal,
  },

  data: () => ({
    title: 'Lion Desk',
    icon: 'add',
  }),
};
</script>

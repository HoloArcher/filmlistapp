<template>
  <v-app>
    <v-container grid-list-xs>
      <v-layout justify-center row wrap>
        <v-flex>
          <!-- <NewEntry /> -->
          <v-btn dark disabled>text</v-btn>
          <app-table v-bind:headers="headers" v-bind:items="items" />
        </v-flex>
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
import NewEntry from "./components/NewEntry";
import Table from "./components/Table";
import axios from 'axios'
import Vue from 'vue'

export default {
  name: "App",
  components: {
    NewEntry,
    "app-table": Table
  },
  async mounted() {
    // this.items = [{ name: "josiah", value: "kek", description: 'why are we still here? Just to suffer? Everyday i can feel my leg, my arm, my fingers. where they used to be' }];

    var obj = await axios.get('/api')
   

    
    Vue.set(this,'items',obj.data.items)
    Vue.set(this,'headers',obj.data.headers)
    
  },
  
  data: () => ({
    kek: '',
    items: [],
    headers: []
  })
};
</script>

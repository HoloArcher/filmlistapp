<template>
  <v-container grid-list-xs>
    <v-data-table
      :items-per-page="100"
      item-key="name"
      show-expand
      single-expand
      :expanded.sync="expanded"
      :search="search"
      :headers="headers"
      :items="items"
      class="elevation-2"
    >
      <template v-slot:item.action="{ item } ">
        <v-btn @click=" item.on = true" icon>
          <v-icon>edit</v-icon>
          <Dialog  v-bind:item="item" />
        </v-btn>
        <v-btn @click="deleteItem(item)" icon>
          <v-icon>delete</v-icon>
        </v-btn>
      </template>
      <template v-slot:expanded-item="{headers}">
        <tr :colspan="headers.length">
          <v-container grid-list-xs>
            <v-layout row wrap>
              <v-flex xs12 v-if="expanded.length > 0 ">{{ expanded[0].name }}</v-flex>
            </v-layout>
          </v-container>
        </tr>
      </template>
    </v-data-table>
  </v-container>
</template>

<script>
import Dialog from "./Dialog";

import axios from "axios";
export default {
  props: ["items", "headers", "search"],
  data() {
    return {
      expanded: []
    };
  },
  components: { Dialog },
  methods: {
    async editItem(item) {
      var result = await axios.put("/api");
      console.log(result);
    },
    async deleteItem(item) {
      var result = await axios.delete("/api");
      console.log(result);
    }
  }
};
</script>

<style>
</style>
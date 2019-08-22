<template>
  <v-container grid-list-xs >
    <v-app-bar app color="primary" >
      <v-spacer></v-spacer>
      <v-flex xs6 offset-xs-2 style="margin: auto">
        <v-text-field 
        v-model="search" 
        solo
        clearable 
        placeholder="Search" 
        >
        </v-text-field>
      </v-flex>
      <v-spacer></v-spacer>
     
    </v-app-bar>
    <v-divider></v-divider>
    <v-data-table
      :items-per-page="100"
      item-key="name"
      show-expand
      single-expand
      :expanded.sync="expanded"
      :search="search"
      :headers="headers"
      :items="items"
      class="elevation-1"
    >
      <template v-slot:expanded-item="{headers}">
        <td :colspan="headers.length">
          <v-container grid-list-xs>
            <v-layout row wrap>
              <v-flex xs12 v-if="expanded.length > 0 ">{{ expanded[0].name }}</v-flex>
            </v-layout>
          </v-container>
        </td>
      </template>
    </v-data-table>
  </v-container>
</template>

<script>
export default {
  props: ["items", "headers"],
  data() {
    return {
      btn: false,
      search: "",
      expanded: []
    };
  }
};
</script>

<style>
</style>
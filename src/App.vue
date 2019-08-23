<template>
  <v-app>
    <v-alert
      dense
      elevation="24"
      transition="slide-y-transition"
      style="z-index: 9999;"
      prominent
      value="true"
      v-model="alert.on"
      dismissible
      :type="alert.type"
    >{{ alert.message}}</v-alert>
    <v-content>
      <v-container grid-list-xs fluid>
        <v-flex>
          <v-app-bar app color="primary">
            <v-flex xs6 style="margin: auto">
              <v-text-field v-model="search" solo clearable placeholder="Search"></v-text-field>
            </v-flex>
            <v-toolbar-items offset-xs1>
              <v-btn text color="success" @click="dialog = !dialog">
                <h2 class="white--text">New entry</h2>
                <v-icon x-large>add</v-icon>
              </v-btn>
            </v-toolbar-items>
          </v-app-bar>
        </v-flex>
        <!-- <v-col cols="2" md="12"> -->
        <!-- <v-col md="12"> -->
        <app-table v-bind:headers="headers" v-bind:items="items" v-bind:search="search" />
        <!-- </v-col> -->
        <!-- </v-col> -->
      </v-container>

      <v-dialog v-model="dialog" persistent max-width="500px">
        <v-card>
          <v-card-title>
            <span class="headline">New entry</span>
          </v-card-title>
          <v-card-text>
            <v-container grid-list-md>
              <v-layout wrap>
                <v-flex xs12>
                  <v-text-field v-model="sub.name" label="Movie name" required></v-text-field>
                </v-flex>

                <v-flex xs12>
                  <div class="text-center">
                    <v-rating v-model="rating"></v-rating>
                    <small>rating</small>
                  </div>
                </v-flex>

                <!-- <v-flex xs12>
                  <v-text-field label="Password" type="password" required></v-text-field>
                </v-flex>-->
                <!-- <v-flex xs12 sm6>
                  <v-select label="Age" required :items="['0-17', '18-29', '30-54', '54+']"></v-select>
                </v-flex>-->
                <!-- <v-flex xs12 sm6>
                  <v-select label="Interests" multiple autocomplete chips :items="['Skiing', 'Ice hockey', 'Soccer', 'Basketball', 'Hockey', 'Reading', 'Writing', 'Coding', 'Basejump']"></v-select>
                </v-flex>-->
              </v-layout>
            </v-container>
            <!-- <small>*indicates required field</small> -->
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn dark color="blue darken-1" text @click.native="dialog = false">Close</v-btn>
            <v-btn dark color="blue darken-1" text @click.native="submit">Save</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-content>
  </v-app>
</template>

<script>
import NewEntry from "./components/NewEntry";
import Table from "./components/Table";
import axios from "axios";
import Vue from "vue";

var error;

export default {
  name: "App",
  components: {
    NewEntry,
    "app-table": Table
  },
  async mounted() {
    // this.items = [{ name: "josiah", value: "kek", description: 'why are we still here? Just to suffer? Everyday i can feel my leg, my arm, my fingers. where they used to be' }];
    try {
      var obj = await axios.get("/api");
    } catch (error) {
      console.trace(error);
      var errobj = {
        on: true,
        type: "error",
        message: error
      };
      Vue.set(this, "alert", errobj);
    }
    if (!error) {
      Vue.set(this, "items", obj.data.items);
      Vue.set(this, "headers", obj.data.headers);
    }
  },

  data: () => ({
    alert: {
      on: true,
      type: "success",
      message: ""
    },
    search: "pulp fiction",
    items: [],
    headers: [],
    dialog: false,
    sub: {
      name: ""
    },
    rating: 0
  }),
  methods: {
    async submit() {
      var obj = {
        name: this.sub.name,
        rating: this.rating
      };
      try {
        var result = await axios.post("/api", obj);
      } catch (error) {
        this.alert.type = "error";
        this.alert["message"] = error;
      }

      this.alert["on"] = true;
      if (result.status == 200) {
        this.items = result.data;
        this.alert.type = "success";
        this.alert["message"] = result.status + " " + result.statusText;
      }
    }
  }
};
</script>

<style lang="css" scoped>
.toolbaritem {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}
</style>
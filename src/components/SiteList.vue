<template>
  <div>
    <v-card max-width="600" class="mx-auto">
      <link href="https://use.fontawesome.com/releases/v5.0.13/css/all.css" rel="stylesheet">
      <v-btn v-on:click="addSite" class="ma-2" tile outlined color="blue">
        <v-icon left dark>mdi-plus</v-icon>ADD
      </v-btn>
      <v-flex xs12>
        <v-hover v-slot:default="{ hover }">
          <v-list>
            <v-list-item v-for="(site) in sites" v-bind:key="site.id" @click>
              <v-list-item-content>
                <v-list-item-title>
                  <a v-bind:href="site.url" target="_blank">{{ site.name }}</a>
                </v-list-item-title>
                <v-list-item-subtitle>{{ site.date | moment }}</v-list-item-subtitle>
              </v-list-item-content>
              <v-list-item-action>
                <v-btn v-on:click="editSite(site.id)" class="ma-2" tile outlined color="green">
                  <v-icon left>mdi-pencil</v-icon>EDIT
                </v-btn>
              </v-list-item-action>
              <v-list-item-action>
                <v-btn v-on:click="deleteSite(site.id)" class="ma-2" tile outlined color="red">
                  <v-icon left>mdi-delete</v-icon>DEL
                </v-btn>
              </v-list-item-action>
            </v-list-item>
          </v-list>
        </v-hover>
      </v-flex>
      <v-card-title>
        localportalV2
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          append-icon="fa-search"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table :headers="headers" :items="sites" :search="search" hide-default-header></v-data-table>
    </v-card>
    <v-data-table
      :headers="headers"
      :items="sites"
      :search="search"
      sort-by="calories"
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat color="white">
          <v-toolbar-title>localportalV2</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-text-field
            v-model="search"
            append-icon="fa-search"
            label="Search"
            single-line
            hide-details
          ></v-text-field>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ on }">
              <v-btn color="primary" dark class="mb-2" v-on="on">New Item</v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="headline">{{ formTitle }}</span>
              </v-card-title>
              <v-card-text>
                <v-container grid-list-md>
                  <v-layout wrap>
                    <v-flex xs12 sm6 md4>
                      <v-text-field v-model="editedItem.name" label="Dessert name"></v-text-field>
                    </v-flex>
                    <v-flex xs12 sm6 md4>
                      <v-text-field v-model="editedItem.calories" label="Calories"></v-text-field>
                    </v-flex>
                    <v-flex xs12 sm6 md4>
                      <v-text-field v-model="editedItem.fat" label="Fat (g)"></v-text-field>
                    </v-flex>
                    <v-flex xs12 sm6 md4>
                      <v-text-field v-model="editedItem.carbs" label="Carbs (g)"></v-text-field>
                    </v-flex>
                    <v-flex xs12 sm6 md4>
                      <v-text-field v-model="editedItem.protein" label="Protein (g)"></v-text-field>
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
                <v-btn color="blue darken-1" text @click="save">Save</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item.action="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)">edit</v-icon>
        <v-icon small @click="deleteItem(item)">delete</v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize">Reset</v-btn>
      </template>
    </v-data-table>
  </div>
</template>

<script>
import moment from "moment";
export default {
  data() {
    return {
      sites: [],
      search: "",
      dialog: false,
      headers: [
        {
          text: "Dessert (100g serving)",
          align: "left",
          sortable: false,
          value: "name"
        },
        { text: "Calories", value: "calories" },
        { text: "Fat (g)", value: "fat" },
        { text: "Carbs (g)", value: "carbs" },
        { text: "Protein (g)", value: "protein" },
        { text: "Actions", value: "action", sortable: false }
      ],
      editedIndex: -1,
      editedItem: {
        name: "",
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0
      },
      defaultItem: {
        name: "",
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0
      }
    };
  },
  created() {
    fetch("http://localhost:3000/sites")
      .then(res => res.json())
      .then(data => {
        this.sites = data;
        console.log(JSON.stringify(this.sites));
      })
      .catch(err => console.log(err));
    this.initialize();
  },
  methods: {
    addSite() {
      this.$router.push("/sites/add");
    },
    editSite(id) {
      this.$router.push(`/sites/edit/${id}`);
    },
    deleteSite(id) {
      fetch(`http://localhost:3000/sites/${id}`, {
        method: "DELETE"
      })
        .then(() => {
          this.sites = this.sites.filter(site => site.id !== id);
        })
        .catch(err => console.log(err));
    },
    goHome() {
      this.$router.push("/");
    },
    initialize() {},
    editItem(item) {
      this.editedIndex = this.sites.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      const index = this.sites.indexOf(item);
      confirm("Are you sure you want to delete this item?") &&
        this.sites.splice(index, 1);
    },

    close() {
      this.dialog = false;
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      }, 300);
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.sites[this.editedIndex], this.editedItem);
      } else {
        this.sites.push(this.editedItem);
      }
      this.close();
    }
  },
  filters: {
    moment: function(date) {
      return moment(date).format("YYYY/MM/DD hh:mm:ss");
    }
  },
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    }
  }
};
</script>
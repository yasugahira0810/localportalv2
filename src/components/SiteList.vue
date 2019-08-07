<template>
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
    <v-text-field v-model="search" append-icon="fa-search" label="Search" single-line hide-details></v-text-field>
    <v-data-table :headers="headers" :items="sites" :search="search"></v-data-table>
  </v-card>
</template>

<script>
import moment from "moment";
export default {
  data() {
    return {
      sites: [],
      search: "",
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
        { text: "Iron (%)", value: "iron" }
      ],
      desserts: [
        {
          name: "Frozen Yogurt",
          calories: 159,
          fat: 6.0,
          carbs: 24,
          protein: 4.0,
          iron: "1%"
        },
        {
          name: "Ice cream sandwich",
          calories: 237,
          fat: 9.0,
          carbs: 37,
          protein: 4.3,
          iron: "1%"
        },
        {
          name: "Eclair",
          calories: 262,
          fat: 16.0,
          carbs: 23,
          protein: 6.0,
          iron: "7%"
        },
        {
          name: "Cupcake",
          calories: 305,
          fat: 3.7,
          carbs: 67,
          protein: 4.3,
          iron: "8%"
        },
        {
          name: "Gingerbread",
          calories: 356,
          fat: 16.0,
          carbs: 49,
          protein: 3.9,
          iron: "16%"
        },
        {
          name: "Jelly bean",
          calories: 375,
          fat: 0.0,
          carbs: 94,
          protein: 0.0,
          iron: "0%"
        },
        {
          name: "Lollipop",
          calories: 392,
          fat: 0.2,
          carbs: 98,
          protein: 0,
          iron: "2%"
        },
        {
          name: "Honeycomb",
          calories: 408,
          fat: 3.2,
          carbs: 87,
          protein: 6.5,
          iron: "45%"
        },
        {
          name: "Donut",
          calories: 452,
          fat: 25.0,
          carbs: 51,
          protein: 4.9,
          iron: "22%"
        },
        {
          name: "KitKat",
          calories: 518,
          fat: 26.0,
          carbs: 65,
          protein: 7,
          iron: "6%"
        }
      ]
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
    }
  },
  filters: {
    moment: function(date) {
      return moment(date).format("YYYY/MM/DD hh:mm:ss");
    }
  }
};
</script>
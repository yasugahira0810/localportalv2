<template>
  <v-app id="keep">
    <v-app-bar app clipped-left color="amber">
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <span class="title ml-3 mr-5">
        Google&nbsp;
        <span class="font-weight-light">Keep</span>
      </span>
      <v-text-field solo-inverted flat hide-details label="Search" prepend-inner-icon="mdi-search"></v-text-field>
      <v-spacer></v-spacer>
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" app clipped color="grey lighten-4">
      <v-list dense class="grey lighten-4">
        <template v-for="(item, i) in items">
          <v-layout v-if="item.heading" :key="i" align-center>
            <v-flex xs6>
              <v-subheader v-if="item.heading">{{ item.heading }}</v-subheader>
            </v-flex>
            <v-flex xs6 class="text-right">
              <v-btn small text>edit</v-btn>
            </v-flex>
          </v-layout>
          <v-divider v-else-if="item.divider" :key="i" dark class="my-4"></v-divider>
          <!-- v-list-item v-else :key="i" @click -->
          <v-list-item v-else :key="i">
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title class="grey--text">{{ item.text }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </v-navigation-drawer>

    <v-content>
      <v-container fluid fill-height class="grey lighten-4">
        <v-layout justify-center align-center>
          <v-flex shrink>
            <div>
              <v-btn v-on:click="addSite" class="ma-2" tile outlined color="blue">
                <v-icon left dark>mdi-plus</v-icon>ADD
              </v-btn>
              <h5>Site List</h5>
              <ul>
                <li v-for="(site) in sites" v-bind:key="site.id">
                  <a v-bind:href="site.url" target="_blank">{{ site.name }}</a>
                  <v-btn v-on:click="editSite(site.id)" class="ma-2" tile outlined color="green">
                    <v-icon left>mdi-pencil</v-icon>EDIT
                  </v-btn>
                  <v-btn v-on:click="deleteSite(site.id)" class="ma-2" tile outlined color="red">
                    <v-icon left>mdi-delete</v-icon>DEL
                  </v-btn>
                </li>
              </ul>
            </div>

            <v-tooltip right>
              <template v-slot:activator="{ on }">
                <v-btn :href="source" icon large target="_blank" v-on="on">
                  <v-icon large>mdi-code-tags</v-icon>
                </v-btn>
              </template>
              <span>Source</span>
            </v-tooltip>
            <v-tooltip right>
              <template v-slot:activator="{ on }">
                <v-btn
                  icon
                  large
                  href="https://codepen.io/johnjleider/pen/zgxbYO"
                  target="_blank"
                  v-on="on"
                >
                  <v-icon large>mdi-codepen</v-icon>
                </v-btn>
              </template>
              <span>Codepen</span>
            </v-tooltip>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
export default {
  props: {
    source: String
  },
  data: () => ({
    drawer: null,
    items: [
      { icon: "mdi-lightbulb_outline", text: "Notes" },
      { icon: "mdi-touch_app", text: "Reminders" },
      { divider: true },
      { heading: "Labels" },
      { icon: "mdi-add", text: "Create new label" },
      { divider: true },
      { icon: "mdi-archive", text: "Archive" },
      { icon: "mdi-delete", text: "Trash" },
      { divider: true },
      { icon: "mdi-settings", text: "Settings" },
      { icon: "mdi-chat_bubble", text: "Trash" },
      { icon: "mdi-help", text: "Help" },
      { icon: "mdi-phonelink", text: "App downloads" },
      { icon: "mdi-keyboard", text: "Keyboard shortcuts" }
    ],
    sites: []
  }),
  created() {
    fetch("http://localhost:3000/sites")
      .then(res => res.json())
      .then(data => {
        this.sites = data;
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
    }
  }
};
</script>

<style>
#keep .v-navigation-drawer__border {
  display: none;
}
</style>
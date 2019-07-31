<template>
  <div>
    <v-btn v-on:click="addSite" class="ma-2" tile outlined color="blue">
      <v-icon left dark>mdi-plus</v-icon>ADD
    </v-btn>
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
</template>

<script>
export default {
  data() {
    return { sites: [] };
  },
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
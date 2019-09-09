<template>
  <v-card max-width="600" class="mx-auto">
    <v-btn v-on:click="goHome()" text icon color="gray">
      <v-icon>mdi-home</v-icon>
    </v-btn>

    <br>
    <input v-model="siteName" placeholder="Site Name">
    <input v-model="siteURL" placeholder="SiteURL">
    <input v-model="siteTags" placeholder="Tags">
    <v-btn v-on:click="editSite()" text icon color="green">
      <v-icon>mdi-check-circle</v-icon>
    </v-btn>
  </v-card>
</template>
<script>
export default {
  data() {
    return {
      siteName: "",
      siteURL: "",
      siteTags: ""
    };
  },
  created() {
    const id = this.$route.params.id;
    fetch(`http://localhost:3000/sites/${id}`)
      .then(res => res.json())
      .then(data => {
        this.siteName = data.name;
        this.siteURL = data.url;
        this.siteTags = data.tags;
      })
      .catch(err => console.log(err));
  },
  methods: {
    editSite() {
      if (this.siteName === "" || this.siteURL === "") return;
      const id = this.$route.params.id;
      const siteInfo = {
        name: this.siteName,
        url: this.siteURL,
        tags: this.siteTags
      };
      fetch(`http://localhost:3000/sites/${id}`, {
        method: "PUT",
        headers: { "Content-type": "application/json" },
        body: JSON.stringify(siteInfo)
      })
        .then(() => {
          this.siteName = "";
          this.siteURL = "";
          this.siteTags = "";
          this.$router.push("/");
        })
        .catch(err => console.log(err));
    },
    goHome() {
      this.$router.push("/");
    }
  }
};
</script>

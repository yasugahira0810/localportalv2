<template>
  <div>
    <router-link to="/">Back</router-link>
    <br>
    <input v-model="siteName" placeholder="Site Name">
    <input v-model="siteURL" placeholder="SiteURL">
    <button v-on:click="editSite()">EDIT</button>
  </div>
</template>
<script>
export default {
  data() {
    return {
      siteName: "",
      siteURL: ""
    };
  },
  created() {
    const id = this.$route.params.id;
    fetch(`http://localhost:3000/sites/${id}`)
      .then(res => res.json())
      .then(data => {
        this.siteName = data.name;
        this.siteURL = data.url;
      })
      .catch(err => console.log(err));
  },
  methods: {
    editSite() {
      if (this.siteName === "" || this.siteURL === "") return;
      const id = this.$route.params.id;
      const siteInfo = { name: this.siteName, url: this.siteURL };
      fetch(`http://localhost:3000/sites/${id}`, {
        method: "PUT",
        headers: { "Content-type": "application/json" },
        body: JSON.stringify(siteInfo)
      })
        .then(() => {
          this.siteName = "";
          this.siteURL = "";
          this.$router.push("/");
        })
        .catch(err => console.log(err));
    }
  }
};
</script>

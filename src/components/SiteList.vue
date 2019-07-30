<template>
  <div>
    <button v-on:click="addSite">ADD</button>
    <h5>Site List</h5>
    <ul>
      <li v-for="(site) in sites" v-bind:key="site.id">
        <a v-bind:href="site.url" target="_blank">{{ site.name }}</a>
        <button v-on:click="editSite(site.id)">EDIT</button>
        <button v-on:click="deleteSite(site.id)">DEL</button>
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

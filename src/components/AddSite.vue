<template>
  <v-card max-width="600" class="mx-auto">
    <v-btn v-on:click="goHome()" text icon color="gray">
      <v-icon>mdi-home</v-icon>
    </v-btn>
    <br>
    <v-form>
      <v-container grid-list-xl>
        <v-layout wrap>
          <v-flex xs12 md4>
            <v-text-field v-model="siteName" label="Site Name" required></v-text-field>
          </v-flex>

          <v-flex xs12 md4>
            <v-text-field v-model="siteURL" label="Site URL" required></v-text-field>
          </v-flex>

          <v-flex xs12 md4>
            <v-btn v-on:click="addSite()" text icon color="green">
              <v-icon>mdi-check-circle</v-icon>
            </v-btn>
          </v-flex>
        </v-layout>
      </v-container>
    </v-form>
  </v-card>
</template>
<script>
export default {
  data() {
    return {
      siteName: "",
      siteURL: ""
    };
  },
  methods: {
    addSite() {
      if (this.siteName === "" || this.siteURL === "") return;
      const siteInfo = {
        name: this.siteName,
        url: this.siteURL,
        date: new Date()
      };
      fetch("http://localhost:3000/sites", {
        method: "POST",
        headers: {
          "Content-type": "application/json"
        },
        body: JSON.stringify(siteInfo)
      })
        .then(() => {
          this.siteName = "";
          this.siteURL = "";
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

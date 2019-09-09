<template>
  <v-card max-width="600" class="mx-auto">
    <v-btn v-on:click="goHome()" text icon color="gray">
      <v-icon>mdi-home</v-icon>
    </v-btn>
    <br>
    <v-form>
      <v-container grid-list-xl>
        <v-layout wrap>
          <v-flex xs12 md3>
            <v-text-field v-model="siteName" label="Site Name" required></v-text-field>
          </v-flex>

          <v-flex xs12 md3>
            <v-text-field v-model="siteURL" label="Site URL" required></v-text-field>
          </v-flex>

          <v-flex xs12 md3>
            <v-text-field v-model="siteTags" label="Tags" required></v-text-field>
          </v-flex>

          <v-flex xs12 md3>
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
import moment from "moment";
export default {
  data() {
    return {
      siteName: "",
      siteURL: "",
      siteTags: ""
    };
  },
  methods: {
    addSite() {
      if (this.siteName === "" || this.siteURL === "") return;
      const siteInfo = {
        name: this.siteName,
        url: this.siteURL,
        tags: this.siteTags,
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
          this.siteTags = "";
          this.$router.push("/");
          console.log(siteInfo);
        })
        .catch(err => console.log(err));
    },
    goHome() {
      this.$router.push("/");
    }
  }
};
</script>

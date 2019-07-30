<template>
    <div>
        <router-link to="/">Back</router-link>
            <br>
            <input v-model="siteName" placeholder="Site Name">
                <input v-model="siteURL" placeholder="Site URL">
                    <button v-on:click="addSite()">ADD</button>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                siteName: '',
                siteURL: ''
            }
        }, methods: {
            addSite() {
                if (this.siteName === '' || this.siteURL === '') return;
                const siteInfo = {
                    name: this.siteName,
                    url: this.siteURL
                }
                fetch('http://localhost:3000/sites', {
                    method: 'POST',
                    headers: {
                        'Content-type': 'application/json'
                    },
                    body: JSON.stringify(siteInfo)
                }).then(() => {
                    this.siteName = '';
                    this.siteURL = '';
                    this.$router.push('/');
                }).catch((err) => console.log(err))
            }
        }
    }
</script>

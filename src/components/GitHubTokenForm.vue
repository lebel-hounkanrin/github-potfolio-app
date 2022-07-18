<template>
  <form @submit.prevent="saveToken">
    <div>
      <label for="githubToken">Github Token</label>
      <br/>
      <input type="text" id="githubToken" v-model="githubToken"/>
    </div>
    <div>
      <input type="submit" value="save token" />
      <button type="button" @click="clearToken"> clear Token</button>
    </div>
  </form>
</template>
<script>
   export default {
    name:"GithubToken",
    data(){
      return {
        githubToken: ""
      }
    },
      beforeMount() {
    this.githubToken = localStorage.getItem("github-token");
  },
    methods: {
      saveToken() {
      localStorage.setItem("github-token", this.githubToken);
    },
      clearToken(){
        localStorage.clear()
      }
    }
   }
</script>
<template>
    <div>
        <h1>Repos</h1>
        <div v-for="repo of repos" :key ="repo.id">
            <h2>{{repo.owner.login}}/{{repo.name}}</h2>
            <RepoIssues :owner="repo.owner" :repo="repo.name" />
        </div>
    </div>
</template>
<script>
import { octokitMixin } from "@/mixins/octokitMixin"
import RepoIssues from "./repo/Issues.vue"
export default {
    name: "Repos",
    componenst: {
        RepoIssues
    },
    data(){
        return{
            repos: []
        }
    },
    mixins: [octokitMixin],
    async mounted() {
        const octokit = this.createOctokitClient();
        const {data: repos} = await octokit.request("/user/repos");
        this.repos = repos;
    },
}
</script>
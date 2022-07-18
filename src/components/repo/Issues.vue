<template>
<div v-if="issues.length > 0">
  <button
  @click="showIssues = !showIssues">{{showIssues ? "Hide": "Show"}} issues</button>
  <div v-if="showIssues">
    <div v-for="issue of issues" :key="issue.id">
      <h3>{{issue.title}}</h3>
      <a :href="issue.url">Go to issue</a>
      <IssueComments :owner="owner" :repo="repo" :issueNumber="issue.number" />
    </div>
  </div>
</div>
</template>
<script>
import { octokitMixin } from '@/mixins/octokitMixin';
import IssueComments from "./issue/Comments.vue"
export default {
  name: "RepoIssues",
  components: {
    IssueComments
  },
  props:{
    owner:{
      type:String, 
      required: true
    },
    repo: {
      type: String,
      required: true
    }
  },
  data(){
    return {
      issues: [],
      showIssues: false
    }
  },
  mixins: [octokitMixin],
  methods:{
    async getRepoIssues(owner, repo){
      const octokit = this.createOctokitClient();
      const {data: issues } = octokit.issues.listForRepo(owner, repo);
      this.issues = issues;
    }
  },
  watch: {
    owner: {
      handler(val){
        this.getRepoIssues(val, this.issues)
      }
    },
    repo: {
      handler(val){
        this.getRepoIssues(this.owner, val)
      }
    }
  },
  created(){
    this.getRepoIssues(this.owner, this.repo)
  }
};
</script>

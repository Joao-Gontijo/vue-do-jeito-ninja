<template>
    <div class="container">
        <div v-if="loader.getIssue" class="spinner-border spinner-border-sm" role="status">
            <span class="sr-only"></span>
        </div>
        <div v-if="!loader.getIssue && issue.number" class="container">
            <h2>Issue #{{issue.number}}</h2>
            <h2>{{issue.title}}</h2>
            <div>{{issue.body}}</div>
            <a href="javascript:history.go(-1)" class="btn btn-primary">Back</a>
            <!-- <button class="btn btn-primary" @click.prevent.stop="clearIssue()">Back</button> -->
        </div>
    </div>    
</template>

<script>

import axios from "axios";

export default {
    name: 'GitHubIssue',
    data(){
        return{
            issue: {},
            loader: {
                getIssue: false
            }
        };
    },
    created(){
        this.getIssue();
    },
    methods:{
        getIssue(issue){
            this.loader.getIssue = true;
            const url = `https://api.github.com/repos/${this.$route.params.name}/${this.$route.params.repo}/issues/${this.$route.params.issue}`;
            axios.get(url).then((response) => {
                this.issue = response.data;
            }).finally(() => {
                this.loader.getIssue = false;
            });
        }
    },
}
</script>

<style>

</style>

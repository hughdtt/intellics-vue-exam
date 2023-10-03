<template>
  <div class="container-fluid">
    <h1 style="color: #4bbb63">Latest Commits</h1>
    <div class="py-2"><button type="button" class="btn btn-success" @click="toggleCommitsDisplay">Toggle Commits</button></div>
    <ul class="list-group">
      <template v-for="{ html_url, sha, author, commit } in commits" :key="html_url">
        <li class="list-group-item p-2">
          <div class="column">
            <div class="col-md-12 d-flex flex-wrap align-items-center">
              <!-- Existing elements like SHA, author, and date here -->
              <a :href="html_url" target="_blank" style="color: #4bbb63" class="commit  mr-1">
                {{ sha.substring(0, 7) }}
              </a>
              <span class="mr-1">By</span><i class="fa fa-fw fa-user"></i>
              <span class="author mr-1">
                <a :href="author.html_url" target="_blank" style="color: #4bbb63" class="font-weight-bold">
                  {{ commit.author.name }}
                </a>
              </span>
              <span class="mr-1">at</span>
              <span class="font-weight-bold mr-1">
                {{ formatDate(commit.author.date) }}
              </span>
            </div>
            <div class="col-md-12" v-if="toggleCommits">
              {{ truncate(commit.message) }}
            </div>
          </div>
        </li>
      </template>
    </ul>
  </div>
</template>

<script>
import moment from "moment";
export default {
  //name: 'component-name',
  //parent: '',
  components: {},
  directives: {},
  filters: {},
  mixins: [],
  props: {},
  data: function () {
    return {
      API_URL:
        "https://api.github.com/repos/vuejs/core/commits?per_page=10&sha=main",
      commits: null,
      toggleCommits: true
    };
  },
  computed: {},
  watch: {},
  beforeCreate: function () { },
  created: function () {
    this.fetchData();
  },
  beforeMount: function () { },
  mounted: function () { },
  beforeUpdate: function () { },
  updated: function () { },
  activated: function () { },
  deactivated: function () { },
  beforeDestroy: function () { },
  destroyed: function () { },
  methods: {
    fetchData: async function () {
      this.commits = await (await fetch(this.API_URL)).json();
    },
    truncate: function (text) {
      const newlineIdx = text.indexOf("\n");
      return newlineIdx > 0 ? text.slice(0, newlineIdx) : text;
    },
    formatDate: function (date) {
      return moment(date).format('DD/MM/YYY h:mm:ssa');
    },
    toggleCommitsDisplay: function(){
      this.toggleCommits = !this.toggleCommits;
    }
  },
};
</script>

<style lang="scss" scoped></style>

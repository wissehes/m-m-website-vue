<template>
  <div>
    <v-dialog v-model="loading" hide-overlay persistent width="300">
      <v-card color="primary" dark>
        <v-progress-linear indeterminate color="white" class="mb-0"></v-progress-linear>
      </v-card>
    </v-dialog>
    <div v-if="!loading" class="article">
        <div class="content">
            <h1 class="content-title">{{ article.title }}</h1>
                    <VueMarkdownIt :source="article.text" />
        </div>
    </div>
  </div>
</template>

<script>
import VueMarkdownIt from "vue-markdown-it";  

import axios from "axios";
export default {
  data() {
    return {
      loading: true,
      article: {},
    };
  },
  components: {
    VueMarkdownIt
  },
  mounted() {
    axios
      .get(`https://api.wissehes.nl/articles/${this.$route.params.id}`)
      .then(res => (this.article = res.data))
      .finally(() => this.loading = false);
  }
};
</script>

<style scoped>
.content {
    margin-left: 10rem;
    margin-right: 10rem;
}
.content-title {
    text-align: center;
}
</style>
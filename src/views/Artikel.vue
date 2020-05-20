<template>
  <div>
    <v-dialog v-model="loading" hide-overlay persistent width="300">
      <v-card color="primary" dark>
        <v-progress-linear
          indeterminate
          color="white"
          class="mb-0"
        ></v-progress-linear>
      </v-card>
    </v-dialog>
    <Error v-if="errored" :load="load"/>
    <v-container v-if="!loading" class="article">
      <div class="content">
        <h1 class="content-title">{{ article.title }}</h1>
        <VueMarkdown>{{ article.text }}</VueMarkdown>
      </div>
    </v-container>
  </div>
</template>

<script>
import VueMarkdown from "vue-markdown";
import Error from "@/components/Error.vue"
import axios from "axios";
export default {
  data() {
    return {
      loading: true,
      errored: false,
      article: {}
    };
  },
  components: {
    VueMarkdown,
    Error
  },
  mounted() {
    this.load()
  },
  methods: {
    load() {
      this.errored = false;
      this.loading = true;
      axios
        .get(`https://api.wissehes.nl/articles/${this.$route.params.id}`)
        .then(res => (this.article = res.data))
        .catch(() => this.errored = true)
        .finally(() => (this.loading = false));
    }
  }
};
</script>

<style>
.content-title {
  text-align: center;
}
.content img {
  max-width: 100%;
}
</style>

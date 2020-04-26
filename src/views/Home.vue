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
    <v-alert prominent type="error" v-if="errored">
      <v-row align="center">
        <v-col class="grow">Kon niet verbinden met de server.</v-col>
        <v-col class="shrink">
          <v-btn @click="load">Opnieuw proberen</v-btn>
        </v-col>
      </v-row>
    </v-alert>
    <div v-if="!loading">
      <v-row no-gutters class="toparticles">
        <v-flex
          v-for="article in topArticles"
          md6
          :key="article._id"
          class="cover"
        >
          <router-link :to="`/artikel/${article._id}`">
            <div
              class="bg"
              :style="
                `background-image:url('https://api.wissehes.nl${article.image.url}')`
              "
            ></div>

            <h1 class="cover-content">{{ article.title }}</h1>
          </router-link>
        </v-flex>
      </v-row>
      <v-layout>
        <v-flex
          v-for="article in articles"
          md4
          :key="article._id"
          class="cover"
        >
          <router-link :to="`/artikel/${article._id}`">
            <div
              class="bg"
              :style="
                `background-image:url('https://api.wissehes.nl${article.image.url}')`
              "
            ></div>

            <h1 class="cover-content">{{ article.title }}</h1>
          </router-link>
        </v-flex>
      </v-layout>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      loading: true,
      errored: false,
      topArticles: [],
      articles: []
    };
  },
  methods: {
    load() {
      this.loading = true;
      this.errored = false;
      axios
        .get("https://api.wissehes.nl/articles")
        .then(res => {
          this.articles = res.data;
          this.topArticles = this.articles.slice(0, 2);
          this.articles = this.articles.slice(2);
          this.errored = false;
        })
        .catch(() => (this.errored = true))
        .finally(() => (this.loading = false));
    }
  },
  mounted() {
    this.load();
  }
};
</script>

<style scoped>
.cover {
  width: 100%;
  height: 50vh;
  overflow: hidden;
  position: relative;
}
.bg {
  width: 100%;
  height: 100%;
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  transition: -webkit-transform 0.5s ease-in-out;
  transition: transform 0.5s ease-in-out;
  transition: transform 0.5s ease-in-out, -webkit-transform 0.5s ease-in-out;
}
.cover:hover .bg {
  transform: scale(1.02);
}
.cover:hover .cover-content {
  -webkit-transform: translateY(-0.2em);
  transform: translateY(-0.2em);
}
.cover-content {
  font-size: 1.35em;
  position: absolute;
  bottom: 0;
  padding: 3vh;
  text-decoration: none;
  transition: -webkit-transform 0.35s ease;
  transition: transform 0.35s ease;
  transition: transform 0.35s ease, -webkit-transform 0.35s ease;
  color: white;
}
</style>

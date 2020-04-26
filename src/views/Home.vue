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
    <Error v-if="errored" :load="load" />
    <transition name="fade">
      <div v-if="!loading">
        <v-row no-gutters class="toparticles" transition="slide-y-transition">
          <v-flex
            v-for="article in topArticles"
            md6
            :key="article._id"
            class="cover"
          >
            <router-link :to="`/artikel/${article._id}`">
              <v-img
                class="bg"
                :src="`https://api.wissehes.nl${article.image.url}`"

              ></v-img>

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
              <v-img
                class="bg"
                :src="`https://api.wissehes.nl${article.image.url}`"
              ></v-img>

              <h1 class="cover-content">{{ article.title }}</h1>
            </router-link>
          </v-flex>
        </v-layout>
      </div>
    </transition>
  </div>
</template>

<script>
import axios from "axios";
import Error from "@/components/Error.vue"

export default {
  data() {
    return {
      loading: true,
      errored: false,
      topArticles: [],
      articles: []
    };
  },
  components: {
    Error
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

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>

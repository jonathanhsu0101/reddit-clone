<template>
  <div class="hello mt-3">
    <ul class="list-unstyled">
      <li v-for="post in posts" class="media m-3" :key="post.id">
        <img :src="post.data.thumbnail" class="mr-3" alt="no picture">
        <div class="media-body">
          <h5 class="mt-0 mb-1"><a :href="createUrl(post.data.permalink)"
           target="_blank">{{post.data.title}}</a></h5>
          <div>
            <h4 class="text-danger">
              {{post.data.ups}} ⏫
            </h4>
            <p>
              created {{formatDate(post.data.created_utc)}} ago by {{post.data.author}}
            </p>
            <span class="badge badge-pill badge-secondary">
              {{post.data.num_comments}} comments
            </span>
            <button
              v-if="isImage(post)"
              @click="post.showImage = !post.showImage"
              type="button"
              class="btn btn-primary">
              {{post.showImage ? 'Hide' : 'Show'}} Image
            </button>
            <div v-if="post.showImage">
              <img :src="post.data.url" alt="">
            </div>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { parse, distanceInWords } from 'date-fns';

export default {
  name: 'Posts',
  data() {
    return {
      posts: [],
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      const url = 'https://www.reddit.com/r/rarepuppers/.json';
      fetch(url)
        .then(response => response.json())
        .then((result) => {
          result.data.children.forEach(child => {
            child.showImage= false;
          })
          this.posts = result.data.children;
        });
    },
    formatDate(date) {
      return distanceInWords(parse(date * 1000), new Date());
    },
    createUrl(path) {
      return `https://www.reddit.com${path}`;
    },
    isImage(post) {
      return post.data.url.match(/\.(jpg|png|jpeg|bmp)$/);
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>

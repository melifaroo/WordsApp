<template>
  <div class="card-deck flex-wrap" style="max-width: 700px; margin: auto;" v-if="blogs && blogs.length">
    <div v-for="blog of currentPageItems" v-bind:key="blog">
      <div class="card bg-light mb-3" style="width=16rem;">
        <div class="card-header">
          <h5 class="card-title">
            <router-link :to="'/blogs/' + blog.blogId">{{blog.url}}</router-link>
          </h5>
        </div>
        <router-link :to="'/blogs/' + blog.blogId"><img
            class="card-img-top"
            :src="'https://loremflickr.com/150/100/' + (word = parseName(blog.url)) + '?lock=' + blog.blogId"
            :alt="'https://loremflickr.com/150/100/' + word + '?lock=' + blog.blogId"/></router-link>
        <div class="card-body">
          <p>
            <router-link class="btn btn-warning" :to="'/blogs/edit/' + blog.blogId">Rename</router-link>
            <router-link class="btn btn-danger" :to="'/blogs/delete/' + blog.blogId">Delete</router-link>
          </p>
        </div>
      </div>
    </div>
  </div>
  <nav v-if="pageCount>1">
    <ul class="pagination justify-content-center">
      <li v-bind:class="(currentPage<2)?'page-item disabled':'page-item'">
        <a class="page-link" @click="currentPage--" tabindex="-1" aria-disabled="true" >Previous</a>
      </li>
     
      <li v-for="page in pageCount" v-bind:key="page" class="page-item"><a class="page-link" @click="currentPage = page">{{page}}</a></li>

      <li v-bind:class="(currentPage>pageCount-1)?'page-item disabled':'page-item'">
        <a class="page-link" @click="currentPage++" tabindex="-1" aria-disabled="true">Next</a>
      </li>
    </ul>
  </nav>
</template>


<script>
export default {
  data() {
    return {
      currentPage: 1,
      perPage: 6,
      paginated_items: [],
      currentPageIndex:0,
    };
  },

  props: {
    blogs: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    pageCount() {
        return Math.ceil(this.blogs.length / this.perPage);
    },
    currentPageItems() {
      return this.blogs.slice( (this.currentPage-1)*this.perPage, Math.min( this.currentPage*this.perPage, this.blogs.length ) );
    },
  },
  methods: {
    parseName(text) {
      const words = text.split(" ");
      return words[Math.floor(Math.random() * words.length)];
    },
  },
};
</script>
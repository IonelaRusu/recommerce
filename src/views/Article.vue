<template>
  <div class="article">
    <span>This is the page for article <i>`{{title}}`</i></span>
    <b-container class="column mt-2">
      <span>This article is written by <b>`{{author}}`</b></span>
      <span>Category: <b>`{{categories}}`</b></span>
      <span>Likes: <b>`{{likes}}`</b></span>
    </b-container>
    <b-container class="mt-4 mb-4 border">
      <span>{{content}}</span>
    </b-container>
    <b-container class="mt-4 mb-4 ">
      <span>Comments:</span>
      <b-row class="comment mt-2 mb-2 border " v-for="comment in comments" :key="comment.id">
        <span> {{comment.body}}</span>
        <br>
        <div><b>Comments Details : </b>
          <span> Comment Likes {{comment.likes}}</span>
        </div>
      </b-row>
    </b-container>

  </div>
</template>


<script>
export default {
  name: 'Article',
  data() {
    return {
      title: '',
      categories: '',
      author: '',
      comments: {},
      likes: 0,
      content: ''

    }
  },
  created() {
    this.id = this.$route.params.details.id;
    this.getComments();
   this.title = this.$route.params.details.title;
   this.author = this.$route.params.details.author;
   this.categories = this.$route.params.details.categories.join(",");
   this.likes = this.$route.params.details.likes;
   this.content = this.$route.params.details.post;
  },
  methods: {
    async getComments() {
      let response = await fetch(`http://fakeapi.jsonparseronline.com/posts/${this.id}/comments`)
          .then(response => response.json())
          .then(json => json);
      this.comments = response;
    }
  }
}
</script>
<style>
.column {
  display:flex;
  flex-direction: column;
}
/*.comment {*/
/*  display: flex;*/
/*  justify-content: left;*/
/*}*/
</style>
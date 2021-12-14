<template>
  <div>
    <posts-table :items="tableItems" :fields="fields"></posts-table>
  </div>
</template>

<script>
// @ is an alias to /src
import PostsTable from '@/components/PostsTable';

export default {
  name: 'Home',
  components: {
    PostsTable,
  },
  data() {
    return {
      posts: null,
      comments: null,
      users: null,
      categories: null,
      tableItems: [],
      fields: ['id', 'title', 'post', 'author', 'categories', 'comments', 'likes', 'actions']
    }
  },
  created() {
    this.initializePage();
  },
  mounted() {
  },
  beforeDestroy() {
  },
  watch: {},
  methods: {
    initializePage() {
      let requestList = [];
      this.getAllPosts().then(() => {
        requestList.push(this.getAllComments())
        requestList.push(this.getAllCategories())
        requestList.push(this.getAllUsers())
        Promise.all(requestList).then(() => this.getFilteredItems())
            .finally(() => {});
      });

    },
    async getAllPosts() {
      let response = await fetch('http://fakeapi.jsonparseronline.com/posts')
          .then(response => response.json())
          .then(json => json)
      this.posts = response;
      this.posts.forEach(post => {
        this.tableItems[post.id] = {
          id: post.id,
          title: post.title,
          post: post.content,
          likes: post.likes,
          comments: 0,
          author: '',
          categories: [],
        }
      });
    },
    async getAllComments() {
      let response = await fetch('http://fakeapi.jsonparseronline.com/comments')
          .then(response => response.json())

      this.comments = response;
      this.posts.forEach(post => {
        let count = 0;
        this.comments.forEach(comment => {
          if (post.id === comment.postId) {
            count++;
          }
        });
        this.tableItems[post.id].comments = count;
      })
    },
    async getAllCategories() {
      let response = await fetch('http://fakeapi.jsonparseronline.com/categories')
          .then(response => response.json());
      this.categories = response;
      this.posts.forEach(post => {
        let categoryList = [];
        this.categories.forEach(category => {
          if (post.categoryId === category.id) {
            categoryList.push(category.name);
          }
        });
        this.tableItems[post.id].categories = categoryList;
      });

    },
    async getAllUsers() {
      let response = await fetch('http://fakeapi.jsonparseronline.com/users')
          .then(response => response.json())
      this.users = response;
      this.posts.forEach(post => {
        let authorName = '';
        this.users.forEach(user => {
          if (post.userId === user.id) {
            authorName = `${user.lastName}  ${user.firstName}`;
          }
        });
        this.tableItems[post.id].author = authorName;
      })
    },
    getFilteredItems() {
      this.tableItems.shift();
    }
  }
}
</script>


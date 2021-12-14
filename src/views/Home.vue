<template>
  <div>
    <posts-table :items="items"></posts-table>
  </div>
</template>

<script>
// @ is an alias to /src
// import {isEmpty} from 'lodash';
import PostsTable from '@/components/PostsTable';
// import { filter } from 'lodash';

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
      items: [],
    }
  },
  // computed: {
  //     items: function() {
  //       console.log(this.tableItems);
  //       console.log(!isEmpty(this.tableItems));
  //       // console.log(Object.values(this.tableItems));
  //       return !isEmpty(this.tableItems) ? this.tableItems : [];
  //   }
  // },
  // watcher: {
  //    tableItems: function() {
  //      this.
  //    }
  // },
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
          post: post.title,
          likes: post.likes,
          comments: 0,
          author: '',
          categories: [],
        }
      });
      console.log(this.tableItems);

    },
    async getAllComments() {
      console.log(this.tableItems);
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


<template>
  <div>
<!--    <b-table striped hover></b-table>-->
    <b-table striped hover :items="items"></b-table>
  </div>
</template>

<script>
// import {filter} from 'lodash';
export default {
  name: 'PostsTable',
  // data() {
  //   return {
  //     items: [
  //       {
  //         id: 40, title: 'Dickerson', post: 'Macdonald', author: 40, categories: 'Dickerson', comments: 'Macdonald',
  //         likes: 'Macdonald', actions: 'Macdonald'
  //       },
  //       {age: 21, first_name: 'Larsen', last_name: 'Shaw'},
  //       {age: 89, first_name: 'Geneva', last_name: 'Wilson'},
  //       {age: 38, first_name: 'Jami', last_name: 'Carney'}
  //     ]
  //   }
  // }
  props: {
      items: {
        required: true,
        type: Array,
      },
  },
  created() {
    // this.items = filter(this.items, item => {
    //   return item !== undefined;
    // })
  }
}
</script>

<style scoped>

</style>
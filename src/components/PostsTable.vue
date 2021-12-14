<template>
  <div>
    <b-table striped hover :items="items" :fields="fields">
      <template #cell(categories)="row">
        {{ row.value.join(',') }}
      </template>
      <template #cell(actions)="row">
        <b-button variant="danger" @click="editPost(row.item, row.index)">Edit</b-button>
      </template>
    </b-table>
  </div>
</template>

<script>
export default {
  name: 'PostsTable',
  props: {
    items: {
      required: true,
      type: Array,
    },
    fields: {
      required: true,
      type: Array,
    },
  },

  methods: {
    editPost(item, index) {
      this.$router.push({
        name: 'Article',
        params: {id: index, details: item}
      });
    },
  }
}
</script>

<style scoped>

</style>
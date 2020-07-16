<template>
<div>
<h1>List</h1>  
  <hr>
  <v-data-table
    v-if="tasks.length"
    :headers="headers"
    :items="tasks"
    show-expand
    :items-per-page="5"
    class="elevation-1"
  >
    <template v-slot:expanded-item="{ item }">
      <td>{{item.description}}</td>
    </template>

    <template v-slot:item.open="{ item }">
      <v-btn color="primary" small :to="'/task/' + item.id">Open</v-btn>
    </template>

  </v-data-table>
  <p v-else>No tasks</p>
</div>
</template>

<script>
export default {
  computed: {
    tasks() {
      return this.$store.getters.tasks
    }
  },
  data () {
    return {
      headers: [
        {
          text: 'Title',
          align: 'start',
          value: 'title',
          sortable: false
        },
        {text: 'Date', value: 'deadline'},
        {text: 'Status', value: 'status'},
        {text: 'Open', value: 'open'},
        { text: '', value: 'data-table-expand' },
      ],
      singleExpand: false,
      expanded: [],
    }
    
  },

}
</script>

<style>

</style>
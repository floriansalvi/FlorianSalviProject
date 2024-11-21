<script lang="ts">
import PouchDb from 'pouchdb';

export default {
  data() {
    return {
      datas: [],
      databaseReference: null
    }
  },

  methods: {
    inc() {},

    initDatabase() {

      const $db = new PouchDb('http://admin:admin@localhost:5984/items')
      if ($db) {
        console.log('valid')
        return
      }
      console.log('invalid')
    },

    fetchData() {
      
    }
  },

  mounted() {
    this.initDatabase()
  }
}
</script>

<template>
  <h1>InfraDon2</h1>
  <p>Counter: {{ datas }}</p>
  <button type="button" role="button" @click="inc">+1</button>
</template>

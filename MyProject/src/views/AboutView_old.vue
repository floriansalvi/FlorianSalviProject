<template>
  <div class="about">
    <h1>This is an about page</h1>
  </div>
</template>
<script lang="ts">
import PouchDB from 'pouchdb';
import { ref } from 'vue';

export default {
  data() {
    return {
      datas: [],
      databaseReference: null as PouchDB.Database | null
    }
  },

  methods: {
    inc() { },

    initDatabase() {
      // lancer une initialisation de la base de données
      // Si la connexion est ok, alors je fais un fetchdata
      const db = new PouchDB('http://admin:admin@localhost:5984/items')
      if (db) {
        console.log('valid');
        this.databaseReference = db
        return
      }
      console.log('invalid');
    },
    fetchData() {
      // Ici je suis dans mon component
      // this.total // access ok 
      const storage = ref(this.databaseReference);
      const self = this; // this = mon composant
      // self = le composant sur je manipule
      if (storage.value) {
        // function de l'objet de ma base de donnéee
        // donc je ne suis plus dans mon composant
        (storage.value).allDocs({
          include_docs: true,
          attachments: true
        }).then(function (result: any) {
          // Une fois la fonction exécuté, il faut que je mette à jour mon modèle
          // hors de mon composant
          // Méthode 1
          // this !== n'est pas mon composant, mais la base de données
          // const self = this
          // en appelant self, je peux mettre à jour mon modèle de données
          console.log('fetchData success =>', result);
          self.datas = result.rows;
          console.log(result.rows)
        }.bind(this)).catch(function (error: any) {
          // méthode 2
          // bind(this)
          // Execute ce code comme si tu étais sur "this" à savoir le composant
          console.log('fetchData error', error);
        });
      }
    },
  },

  mounted() {
    this.initDatabase();
    this.fetchData();
  }
}
</script>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>

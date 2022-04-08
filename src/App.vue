<script>
import contacts from './contacts.json';

export default {
  data() {
    return {
      contacts: contacts,
      shownContacts: [],
      sortingMethod: "name"
    }
  },
  created() {
    this.shownContacts = this.contacts.splice(0,5);
  },
  methods: {
    addRandomContact() {
      if (this.contacts.length == 0) return;

      const randomIndex = Math.floor(Math.random() * this.contacts.length);
      this.shownContacts.push( this.contacts[randomIndex] );
      this.contacts.splice(randomIndex, 1);
    },
    sortByName() {
      this.sortingMethod = "name";

      this.shownContacts.sort((a, b) => {
        if (a.name < b.name) return -1;
        if (a.name > b.name) return 1;
        return 0;
      });
    },
    sortByPopularity() {
      this.sortingMethod = "popularity";

      this.shownContacts.sort((a, b) => b.popularity - a.popularity);
    },
    deleteContact(id) {
      const index = this.shownContacts.findIndex(c => c.id == id);

      this.contacts.push( this.shownContacts[index] );
      this.shownContacts.splice(index, 1);
    }
  },
  computed: {
    sortedContacts() {
      if (this.sortingMethod == "name") this.sortByName();
      if (this.sortingMethod == "popularity") this.sortByPopularity();
      
      return this.shownContacts;
    }
  }
}
</script>

<template>
  <div id="app">
    <div class="buttons">
      <button @click="addRandomContact">Add random contact</button>
      <button @click="sortByName">Sort by Name</button>
      <button @click="sortByPopularity">Sort by Popularity</button>
    </div>
    <table>
      <thead>
        <th>Picture</th>
        <th>Name</th>
        <th>Popularity</th>
        <th>Won an Oscar</th>
        <th>Won an Emmy</th>
        <th>Actions</th>
      </thead>
      <tbody>
        <tr v-for="contact in sortedContacts" :key="contact.id">
          <td><img :src="contact.pictureUrl" :alt="contact.name"></td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity.toFixed(2) }}</td>
          <td>{{ contact.wonOscar ? '&#127942;' : '' }}</td>
          <td>{{ contact.wonEmmy ? '&#127942;' : '' }}</td>
          <td><button @click="deleteContact(contact.id)">Delete</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 20px;

  color: #2c3e50;
  margin-top: 60px;
}

.buttons {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
}

/* button {
  
} */

img {
  width: 70px;
}
</style>

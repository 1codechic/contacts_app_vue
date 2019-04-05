<template>
  <div class="home">
    <h1>All Contacts</h1>
    <div <div v-for="contact in contacts">
      <p>{{ contact.name }}</p>
      <p>{{ contact.email }}</p>
      <p>{{ contact.phone_number }}</p>
      <p>{{ contact.bio}}</p>
      <hr>

    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import axios from "axios";

export default {
  data: function() {
    return {
      contacts: []
    };
  },
  created: function() {
    axios.get("/api/contacts").then(response => {
      this.contacts = response.data;
    });
  },
  methods: {}
};
</script>


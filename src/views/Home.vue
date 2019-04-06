<template>
  <div class="home">
    <h1>All Contacts</h1>
    <!-- New/Create Action -->
    <div>
     <p>First Name: <input type ="text" v-model="newContactFirstName"></p> 
     <p>Middle Name: <input type ="text" v-model="newContactMiddleName"></p> 
     <p>Last Name: <input type ="text" v-model="newContactLastName"></p> 
     <p>Email: <input type ="text" v-model="newContactEmail"></p> 
     <p>Phone Number: <input type ="text" v-model="newContactPhoneNumber"></p> 
     <p>BIO: <input type ="text" v-model="newContactBIO"></p> 
     <button v-on:click="createContact()">Create Contact</button>
     <hr>
    </div>


    <!-- Index Action-->
    <div v-for="contact in contacts">
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
      contacts: [],
      newContactFirstName: "",
      newContactMiddleName: "",
      newContactLastName: "",
      newContactEmail: "",
      newContactPhoneNumber: "",
      newContactBIO: ""
    };
  },
  created: function() {
    axios.get("/api/contacts").then(response => {
      this.contacts = response.data;
    });
  },
  methods: {
    createContact: function() {
      var params = {
        first_name: this.newContactFirstName,
        middle_name: this.newContactMiddleName,
        last_name: this.newContactLastName,
        email: this.newContactEmail,
        phone_number: this.newContactPhoneNumber,
        bio: this.newContactBIO
      }
      axios.post("/api/contacts", params).then(response => {
        console.log(response);
        this.contacts.push(response.data);
      });
    }
  }
};
</script>


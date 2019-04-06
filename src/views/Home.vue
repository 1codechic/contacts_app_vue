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
    <!-- end New/Create Action -->


    <!-- Index Action-->
    <div v-for="contact in contacts">
      <!-- end of index -->
      <p>{{ contact.name }}</p>
      <!-- show more info -->
      <button v-on:click="toggleContact(contact)">Show more</button>
      <div v-if="contact === currentContact">
        <p>{{ contact.email }}</p>
        <p>{{ contact.phone_number }}</p>
        <p>{{ contact.bio}}</p>
      <!-- end of more info -->
      <!-- edit contact -->
        <p>First Name: <input type ="text" v-model="contact.first_name"></p> 
        <p>Middle Name: <input type ="text" v-model="contact.middle_name"></p> 
        <p>Last Name: <input type ="text" v-model="contact.last_name"></p> 
        <p>Email: <input type ="text" v-model="contact.email"></p> 
        <p>Phone Number: <input type ="text" v-model="contact.phone_number"></p> 
        <p>BIO: <input type ="text" v-model="contact.bio"></p>
        <button v-on:click="updateContact(contact)">Update Contact</button>
        <button v-on:click="deleteContact(contact)">Delete Contact</button> 
      </div>
      <!-- end edit contact -->
      <!-- end delete contact -->
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
      newContactBIO: "",
      currentContact: {}
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
    },
    toggleContact: function (theContact) {
      if (this.currentContact === theContact) {
        this.currentContact = {};
      }
      else {
        this.currentContact = theContact;
      }
    },
    updateContact: function(theContact) {
      var params = {
        first_name: theContact.first_name,
        middle_name: theContact.middle_name,
        last_name: theContact.last_name,
        email: theContact.email,     
        phone_number: theContact.phone_number,
        bio: theContact.bio
    };
    axios.patch("/api/contacts/" + theContact.id, params).then(response => {
      console.log(response);
      theContact = response.data;
      })
    } ,
    deleteContact: function(theContact) {
      axios.delete("/api/contacts/" + theContact.id).then(response => {
        console.log(response);
        var index = this.contacts.indexOf(theContact);
        this.contacts.splice(index, 1);
      })
    }
  }
};
</script>


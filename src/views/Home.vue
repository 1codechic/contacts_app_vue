<template>
  <div class="home">
    <h1>All Contacts</h1>
    <!-- New/Create Action -->
    <div>
     <p>First Name: <input type ="text" v-model="newContact.firstName"></p> 
     <p>Middle Name: <input type ="text" v-model="newContact.middleName"></p> 
     <p>Last Name: <input type ="text" v-model="newContact.lastName"></p> 
     <p>Email: <input type ="text" v-model="newContact.email"></p> 
     <p>Phone Number: <input type ="text" v-model="newContact.phoneNumber"></p> 
     <p>BIO: <input type ="text" v-model="newContact.bio"></p> 
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
        <p>First Name: <input type ="text" v-model="contact.firstName"></p> 
        <p>Middle Name: <input type ="text" v-model="contact.middleName"></p> 
        <p>Last Name: <input type ="text" v-model="contact.lastName"></p> 
        <p>Email: <input type ="text" v-model="contact.email"></p> 
        <p>Phone Number: <input type ="text" v-model="contact.phoneNumber"></p> 
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
      newContact: {
        firstName: "",
        middleName: "",
        lastName: "",
        email: "",
        phoneNumber: "",
        bio: ""
      },
      currentContact: {}
    };
  },
  created: function() {
   axios.get("/api/contacts").then(response => {
      console.log(response);
      this.contacts = response.data;
    });
  },
  methods: {
    createContact: function() {
      var params = {
        first_name: this.newContact.firstName,
        middle_name: this.newContact.middleName,
        last_name: this.newContact.lastName,
        email: this.newContact.email,
        phone_number: this.newContact.phoneNumber,
        bio: this.newContact.bio
      }
      axios.post("/api/contacts", params).then(response => {
        console.log(response);
        this.contacts.push(response.data);
      });
    },
    toggleContact: function (contact) {
      if (this.currentContact === contact) {
        this.currentContact = {};
      }
      else {
        this.currentContact = contact;
      }
    },
    updateContact: function(contact) {
      var params = {
        first_name: contact.firstName,
        middle_name: contact.middleName,
        last_name: contact.lastName,
        email: contact.email,     
        phone_number: contact.phoneNumber,
        bio: contact.bio
      };
      axios.patch("/api/contacts/" + contact.id, params).then(response => {
      console.log(response);
      contact = response.data;
      });
    } ,
    deleteContact: function(contact) {
      axios.delete("/api/contacts/" + contact.id).then(response => {
        console.log("Deleting Contact");
        console.log(response);
        console.log("contact deleted")
        var index = this.contacts.indexOf(contact);
        this.contacts.splice(index, 1);
      })
    }
  }
};
</script>


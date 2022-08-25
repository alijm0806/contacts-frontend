<script>
import axios from "axios"
import { assertExpressionStatement } from '@babel/types';

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      contacts: [],
      newContact: {},
      currentContact: {},
      name: "ALI"
    };
  },
  created: function () {
    this.indexContacts(),
      this.indexContacts()
  },
  methods: {
    indexContacts: function () {
      console.log("all contacts...");
      axios.get("http://localhost:3000/contacts").then(response => {
        console.log(response.data);
        this.contacts = response.data
      })
    },
    createContact: function () {
      console.log("new contact...");
      axios.post("http://localhost:3000/contacts", this.newContact).then(response => {
        console.log(response.data);

        this.contacts.push(response.data);
      })
    },
    showContact: function (currentContact) {
      console.log("showing contact...");
      axios.get("http://localhost:3000/contacts/" + currentContact.id).then(response => {
        console.log(response.data);

        this.currentContact = response.data;
      })
      document.querySelector('#contact-details').showModal();
    },
    updateContact: function (currentContact) {
      console.log("update contact...");
      console.log(this.currentContact);
      axios.patch("http://localhost:3000/contacts/" + currentContact.id, this.currentContact).then(response => {
        console.log(response.data);


        this.currentContact = {}
      })
    },
    deleteContact: function (currentContact) {
      console.log("update contact...");
      console.log(this.currentContact);
      axios.delete("http://localhost:3000/contacts/" + currentContact.id).then(response => {
        console.log(response.data);
        var index = this.contacts.indexOf(currentContact);
        this.contacts.splice(index, 1);
      })
    }
  }
}

</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h1>{{ name }}</h1>
    <p><b>First Name: </b> <input type="text" v-model="newContact.first_name"></p>
    <p><b>Last Name: </b> <input type="text" v-model="newContact.last_name"></p>
    <p><b>Email: </b> <input type="text" v-model="newContact.email"></p>
    <p><b>Phone Number: </b> <input type="text" v-model="newContact.phone_number"></p>
    <p><b>Image: </b> <input type="text" v-model="newContact.image"></p>
    <button v-on:click="createContact()">New Contact</button>


    <div v-for=" contact in contacts">
      <p>{{ contact.first_name }}</p>
      <p>{{ contact.last_name }}</p>
      <p>{{ contact.email }}</p>
      <p>{{ contact.phone_number }}</p>
      <img v-bind:src="contact.image" width="250" height="300" />
      <br>
      <!-- <button v-on:click="indexRecipes()">get data</button> -->
      <button v-on:click="showContact(contact)">More Info</button>
      <br>
    </div>

    <dialog id="contact-details">
      <form method="dialog">
        <p><b>First Name: </b>{{ currentContact.first_name }}</p>
        <p><b>Last Name: </b>{{ currentContact.last_name }}</p>
        <p><b>Email: </b>{{ currentContact.email }}</p>
        <p><b>Phone Number: </b>{{ currentContact.phone_number }}</p>
        <p><b>Image: </b>{{ currentContact.image }}</p>
        <hr />
        <hr />
        <p><b>First Name: </b> <input type="text" v-model="currentContact.first_name"></p>
        <p><b>Last Name: </b> <input type="text" v-model="currentContact.last_name"></p>
        <p><b>Email: </b> <input type="text" v-model="currentContact.email"></p>
        <p><b>Phone Number: </b><input type="text" v-model="currentContact.phone_number"></p>
        <p><b>Image: </b><input type="text" v-model="currentContact.image"></p>
        <button v-on:click="updateContact(currentContact)">Update Contact</button>
        <button v-on:click="deleteContact(currentContact)">Delete Contact</button>
        <button>Close</button>
      </form>
    </dialog>

  </div>
</template>

<style>
</style>
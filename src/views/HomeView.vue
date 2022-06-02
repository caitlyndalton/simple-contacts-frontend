<script>
import axios from "axios";

export default {
  data: function () {
    return {
      contacts: [],
      currentContact: {},
      newContactParams: {},
      editContactParams: {},
    };
  },
  created: function () {
    this.indexContacts();
  },
  methods: {
    indexContacts: function () {
      axios.get("/contacts").then((response) => {
        console.log("contacts index", response);
        this.contacts = response.data;
      });
    },
    createContact: function () {
      axios
        .post("/contacts", this.newContactParams)
        .then((response) => {
          console.log("contacts create", response);
          this.contacts.push(response.data);
          this.newContactParams = {};
        })
        .catch((error) => {
          console.log("contacts create error", error.response);
        });
    },
    showContact: function (contact) {
      console.log("show the contact", contact);
      this.currentContact = contact;
      this.editContactParams = contact;
      document.querySelector("#contact-details").showModal();
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>All Contacts</h1>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <h2>{{ contact.first_name + " " + contact.last_name }}</h2>
      <img v-bind:src="contact.image" v-bind:alt="contact.name" />
      <p>{{ contact.email }}</p>
      <p>{{ contact.phone_number }}</p>
      <div>
        <button v-on:click="showContact(contact)">Show more</button>
      </div>
    </div>
    <dialog id="contact-details">
      <form method="dialog">
        <h1>Contact Info</h1>
        <p>Name: {{ currentContact.first_name + " " + currentContact.last_name }}</p>
        <p>Email: {{ currentContact.email }}</p>
        <p>Phone Number: {{ currentContact.phone_number }}</p>
      </form>
    </dialog>
  </div>
</template>

<style>
.home img {
  width: 100px;
}
</style>

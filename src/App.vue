<template>
  <div class="container">
    <h1>Iron contacts</h1>
    <button @click="addRandomContact">Add Random Contact</button>
    <button @click="sortAlphabetically">Sort by Name</button>
    <button @click="sortByPopularity">Sort by Popularity</button>
    <table>
      <tbody>
        <tr>
          <th>Picture</th>
          <td>Name</td>
          <td>Popularity</td>
          <td>Won Oscar</td>
          <td>Won Emmy</td>
          <td>Actions</td>
        </tr>
        <tr v-for="(contact, index) in sortedContactList" :key="index">
          <td><img :src="contact.pictureUrl" alt="" /></td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity }}</td>
          <td v-if="contact.wonOscar === true">🏆</td>
          <td v-else></td>
          <td v-if="contact.wonEmmy === true">&#11088</td>
          <td v-else></td>
          <td><button @click="deleteContact(index)">Delete</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref } from "vue";
import contacts from "./contacts.json";

let sortContactList = ref(contacts.slice(0, 5));

function addRandomContact() {
  const randomContactIndex = Math.floor(Math.random() * contacts.length);
  const randomContact = contacts[randomContactIndex];

  const isContactAlreadyAdded = sortContactList.value.some(
    (contact) => contact.id === randomContact.id
  );

  if (
    !isContactAlreadyAdded &&
    sortContactList.value.length < contacts.length
  ) {
    sortContactList.value.push(randomContact);
  } else if (sortContactList.value.length === contacts.length) {
    alert("No more contacts available to add.");
  } else {
    const nextRandomContact = contacts.find(
      (contact) =>
        !sortContactList.value.some(
          (addedContact) => addedContact.id === contact.id
        )
    );
    sortContactList.value.push(nextRandomContact);
  }


  let sortNameStatus = ref(true);
function sortAlphabetically() {
  if (sortNameStatus) {
    sortContactList.value.sort((a, b) => a.name.localeCompare(b.name));
    sortNameStatus = false;
  } else {
    sortContactList.value.sort((a, b) => b.name.localeCompare(a.name));
    sortNameStatus = true;
  }
}

let sortPopularityStatus = ref(true);
function sortByPopularity() {
  if (sortPopularityStatus) {
    sortContactList.value.sort((a, b) => a.popularity - b.popularity);
    sortPopularityStatus = false;
  } else {
    sortContactList.value.sort((a, b) => b.popularity - a.popularity);
    sortPopularityStatus = true;
  }
}

function deleteContact(index) {
  sortContactList.value.splice(index, 1);
}

</script>

<style>
.container {
  text-align: center;
  align-content: center;
  font-weight: bold;
}
table {
  margin: 30px 30px;
}
td {
  padding: 30px 30px;
  font-size: 12px;
}
img {
  width: 120px;
}
</style>
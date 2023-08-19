<template>
    <div>
        <h1>IronContacts</h1>
        <div class="buttons">
            <button @click="addRandomContact">Add Random Contact</button>
            <button @click="orderByPopularity">Sort by popularity</button>
            <button @click="orderByName">Sort by name</button>
        </div>
        <v-table>
            <thead>
                <tr>
                    <th>Picture</th>
                    <th>Name</th>
                    <th>Popularity</th>
                    <th class="prized">Won Oscar</th>
                    <th class="prized">Won Emmy</th>
                    <th class="prized">Actions</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="contact in contactsListed">
                    <td><img :src="contact.pictureUrl"></td>
                    <td>{{ contact.name }}</td>
                    <td>{{ contact.popularity.toFixed(2) }}</td>
                    <td>{{ contact.wonOscar ? '🏆' : '' }}</td>
                    <td>{{ contact.wonEmmy ? '🏆' : '' }}</td>
                    <td><button @click="removeContact(contact.id)">Delete</button></td>
                </tr>
            </tbody>
        </v-table>
    </div>
</template>
  
<script setup>
    import { reactive } from "vue";
    import contactsData from '../assets/contacts.json'
    const contacts = reactive(contactsData);
    let contactsListed = reactive(contacts.slice(5,10));
    
    function removeContact(id) {
        if (contactsListed.length > 0) {
            const index = contactsListed.findIndex(contact => contact.id === id);
            contactsListed.splice(index, 1);// = contactsListed.filter(contact => contact.id !== id);

        }
        
    }

    function addRandomContact() {
        let randomIndex = Math.floor(Math.random() * contacts.length + 1);
        let contactSelected = contacts.at(randomIndex);
        if (!contactsListed.includes(contactSelected)){
            contactsListed.push(contacts.at(randomIndex));
        }
        
    }

    function orderByPopularity() {
        return contactsListed.sort((a, b) => {
            if (a.popularity > b.popularity) return -1;
            else if (a.popularity < b.popularity) return 1;
            else return 0
        });
    }

    function orderByName() {
        return contactsListed.sort((a, b) => {
            if (a.name > b.name) return 1;
            else if (a.name < b.name) return -1;
            else return 0
        });
    }

</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat&family=Roboto:wght@500&display=swap');

* {
    font-family: 'Roboto', sans-serif;
    font-weight: normal;
    text-align: center;
}

.buttons {
    display: flex;
    justify-content: center;
    gap: 2rem;
    
    
}

img {
    width: 100px;
}

.prized {
    width: 50px;
}
</style>
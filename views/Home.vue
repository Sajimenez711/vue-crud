import EditModal from '@/compone/EditModal';
<template>
<div>
    <v-container >
        <v-row >
             <v-col
          v-for="card in cards"
          :key="card.title"
          :cols="card.flex"
        >
            <v-card max-width=300 max-height=400 >
                <v-title>{{ card.first_name }} {{ card.last_name }}</v-title>
                <v-img contain  :src="card.avatar">
                </v-img>
                <v-card-actions>
              <v-spacer>
              </v-spacer>
                <v-tooltip bottom>
                <template v-slot:activator="{ on }">
                                <v-btn v-on="on"  v-on:click="viewUser(card.id)" icon>
                                    <EditModal v-bind:icon="viewIcon" />
                                </v-btn>
                    </template>
                    <span>View User</span>
                </v-tooltip>
              <v-tooltip bottom>
                <template v-slot:activator="{ on }">
                                <v-btn v-on="on"  v-on:click="editUser(card.id)" icon>
                                    <EditModal v-bind:icon="editIcon"/>
                                </v-btn>
                    </template>
                    <span>Edit User</span>
                </v-tooltip>
                <v-tooltip bottom>
                <template v-slot:activator="{ on }">
                        <v-btn icon>
                            <v-icon  color="primary" v-on="on" v-on:click="deleteUser(card.id)">delete</v-icon>
                        </v-btn>
                    </template>
                    <span>Delete User</span>
                </v-tooltip>
              
            </v-card-actions>
            </v-card>

     </v-col>
    </v-row>
    </v-container>
</div>
    
</template>
<script>
import EditModal from '@/views/EditModal'
import axios from 'axios'
export default {
    components: { EditModal },
    data:() => ({
        cards: [],
        editIcon: 'edit',
        viewIcon: 'visibility',
        something: true,
    }),
    methods: {
        getAllUsers() {
            axios
            .get('https://reqres.in/api/users?page=1')
            .then(response => (this.$store.state.users = response.data.data, this.cards = this.$store.state.users))
        },
        deleteUser(id) {
            axios
            .delete(`https://reqres.in/api/users/${id}`)
            .then(() => {
                const deletedUser = this.cards.findIndex(card => card.id === id)
                this.cards.splice(deletedUser, 1)
            })
        },
        editUser(id) {
            this.$store.state.userId = id
            this.$store.state.saveDisabled = false

        },
        viewUser(id) {
            const selectedUser = this.cards.findIndex(card => card.id === id)
            this.$store.state.userId = id
            this.$store.state.user = this.cards[selectedUser]
            this.$store.state.saveDisabled = true
        }
    },
    mounted() {
        this.getAllUsers()
    },
    
}
</script>
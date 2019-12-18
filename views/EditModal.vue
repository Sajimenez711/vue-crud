<template>
  <v-row justify="center">
    <v-dialog v-model="dialog" persistent max-width="600px">
      <template v-slot:activator="{ on }">
        <v-btn  color="secondary" dark v-on="on" icon>
          <v-icon>{{ icon }}</v-icon>
        </v-btn>
      </template>
      <v-card>
        <v-card-title>
          <span class="headline">User Profile</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12" sm="6" md="4">
                <v-text-field v-model="first_name" :disabled="$store.state.saveDisabled" label="Legal first name*" required></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="4">
                <v-text-field 
                v-model="last_name"
                :disabled="$store.state.saveDisabled"
                  label="Legal last name*"
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field v-model="email" :disabled="$store.state.saveDisabled" label="Email*" required></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field v-model="avatar" :disabled="$store.state.saveDisabled" label="Avatar*" required></v-text-field>
              </v-col>
            </v-row>
          </v-container>
          <small :hidden="$store.state.saveDisabled" >*indicates required field</small>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text v-on:click="clean" >Close</v-btn>
          <v-btn color="blue darken-1" text :hidden="$store.state.saveDisabled" v-on:click="upsertUser($store.state.userId)">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>
<script>
import axios from 'axios'
  export default {
    props: {
      icon: String,
       disable: Boolean
    },
    data () {
      return {
        dialog: false,
        first_name: '',
        last_name: '',
        email: '',
        editedUser: null,
        avatar: '',
      }
    },
    methods: {
      upsertUser (id)  {
        this.editedUser = this.$store.state.users.findIndex(card => card.id === id);
        if(this.editedUser != -1) {
        axios
        .put(`https://reqres.in/api/users/${id}`)
        .then(() => {
          this.$store.state.users[this.editedUser].first_name = this.first_name;
          this.$store.state.users[this.editedUser].last_name = this.last_name;
          this.$store.state.users[this.editedUser].email = this.email;
          this.$store.state.users[this.editedUser].avatar = this.avatar;

          this.dialog = false
      }) 
      } else {
        axios
         .post(`https://reqres.in/api/users`)
        .then((response) => {
          const newUser = { 
            id: response.data.id,
            first_name: this.first_name,
          last_name: this.last_name,
          email: this.email,
          avatar: this.avatar,
          }
         
          this.$store.state.users.push(newUser);
          this.dialog = false
      })  
      }
      },
      getUserInfo () {
        this.first_name = this.$store.state.user.first_name
        this.last_name = this.$store.state.user.last_name
        this.email = this.$store.state.user.email
        this.avatar = this.$store.state.user.avatar
      },
      clean() {
        this.dialog = false;
        this.$store.state.saveDisabled = false
      }
    },
    mounted() {
      this.btnIcon = this.icon
    },
    beforeUpdate() {
        if(this.$store.state.saveDisabled) {
          this.getUserInfo()
      }
    }
  }
</script>
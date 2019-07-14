<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8>
      <v-card min-width="400">
        <v-snackbar v-model="snackbar" :timeout="6000" top>{{ message }}
          <v-btn color="pink" flat @click="snackbar = false">Close</v-btn>
        </v-snackbar>


        <v-card-title>
          <h1>Nuxt chat</h1>
        </v-card-title>

        <v-card-text>
          <v-form ref="form" v-model="valid" lazy-validation>

            <v-text-field v-model="name" :counter="20" :rules="nameRules" label="Your Name" required></v-text-field>

            <v-text-field v-model="room" :rules="roomRules" label="Please enter room" required></v-text-field>

            <v-btn :disabled="!valid" color="success" @click="submit">Sign in</v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>


<script>
    import {mapMutations} from 'vuex'

    export default {
        layout: 'empty',
        head: {
            title: 'Main page'
        },
        data: () => ({
            valid: true,
            snackbar: false,
            message: '',
            name: '',
            room: '',
            nameRules: [
                v => !!v || 'Name is required',
                v => (v && v.length <= 20) || 'Name must be less than 20 characters'
            ],
            roomRules: [
                v => !!v || 'Room is required'
            ]
        }),
        mounted () {
            const  { message } = this.$route.query;
            if (message === 'noUser') {
                this.message = "Enter data"
            } else if (message === 'leftChat') {
                this.message = "You left this chat"
            }
            this.snackbar = !!this.message

        },

        methods: {
            ...mapMutations (['setUser']),
            submit () {
                if (this.$refs.form.validate()) {
                  const user = {
                      name: this.name,
                      room: this.room
                  };
                  this.$socket.emit('userJoined', user, data => {
                      if (typeof data === 'string') {
                          console.error(data)
                      } else {
                          user.id = data.userId;
                          this.setUser(user);
                          this.$router.push('./chat')
                      }
                  });
                }
            },
        }
    }
</script>


<template>
  <v-container fluid="true">
    <navMenu></navMenu>
  <br>
    <div>
      <v-spacer></v-spacer>
      <v-layout>
        <v-flex>
          <h2 class="text-md-center mb-4 red--text">
            Courses
          </h2>
        </v-flex>

      </v-layout>

      <v-layout row wrap>
        <v-flex xs12 sm5 offset-sm3>
          <v-card class="light-blue lighten-4 mb-4" v-for="item in items" :key="item._id">
            <v-container fluid
                         grid-list-lg>
              <v-card-title primary-title>
                <div>
                  <div> Course Subject: {{ item.course_subject }}</div>
                </div>
              </v-card-title>

              <v-card-actions>
                <v-btn flat class="green white--text" v-bind:to= "{name: 'adminUpdateCourse', params: {id: item._id }}">Edit</v-btn>
                <v-btn flat class="green white--text" @click="deleteData(item._id)"> Delete</v-btn>
              </v-card-actions>



            </v-container>

          </v-card>
        </v-flex>
      </v-layout>
    </div>

  </v-container>

</template>



<script>
  import Axios from 'axios'
  import Vue from 'vue'
  import Menu from '@/components/admin/menu'
  export default {
    name: 'app',
    components: {'navMenu': Menu},
    data () {
      return {
        sideNav: false,
        items: {
          _id: '',
          course_subject: ''
        },
        right: null

      }
    },
    created: function () {
      console.log(Vue.localStorage.get('token'))
      var jwt = Vue.localStorage.get('token')
      if (jwt) {
        Axios.get('http://192.168.137.1:3000/admin/course/view', {
          headers: {
            'Authorization': 'bearer ' + Vue.localStorage.get('token')
          }
        })
          .then((response) => {
            this.items = response.data
            console.log('it works')
          })
          .catch((error) => {
            console.log(error)
          })
      } else {
        this.$router.push('/admin/login')
      }
    },
    methods: {
      deleteData: function (_id) {
        console.log(Vue.localStorage.get('token'))
        var jwt = Vue.localStorage.get('token')
        if (jwt) {
          Axios.delete('http://192.168.137.1:3000/admin/course/remove/' + _id, {
            headers: {
              'Authorization': 'bearer ' + Vue.localStorage.get('token')
            }
          })
          .then(res => {
            console.log(res)
            console.log('it works')
          })
          .catch(function (error) {
            console.log(error)
          })
          this.$router.push('/admin/course/remove/')
        } else {
          this.$router.push('/admin/login')
        }
      }
    }
  }
</script>


<template>
  <v-container fluid="true">
    <navMenu></navMenu>
    <v-spacer></v-spacer>
    <v-layout>
      <v-flex>
        <h2 class="text-md-center mb-5 red--text">
         Enquiries
        </h2>
      </v-flex>
    </v-layout>
  <v-data-table
    :headers="headers"
    :items="items"
    v-for="item in items"
    :key="item._id"
    class="elevation-1"
  >
    <template slot="items" slot-scope="props">
      <td>{{ props.item.inquiry_email }}</td>
      <td class="text-xs-left"><b>{{ props.item.inquiry_title }}</b></td>
      <td> <v-btn :key="item._id"  v-bind:to= "{name: 'AdminviewInquiry', params: {id: item._id } }" class="green lighten-2 white--text"> View</v-btn></td>
    </template>
  </v-data-table>
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
        headers: [
          {
            text: 'Email ID',
            align: 'left',
            sortable: false,
            value: 'inquiry_email'
          },
          { text: 'Date', value: 'inquiry_date' },
          { text: 'Subject', value: 'inquiry_title' }
        ],
        items: [{
          value: false,
          _id: '',
          inquiry_date: '',
          inquiry_title: '',
          inquiry_email: ''
        }],
        sideNav: false,
        right: null
      }
    },
    created: function () {
      console.log(Vue.localStorage.get('token'))
      var jwt = Vue.localStorage.get('token')
      if (jwt) {
        Axios.get('http://192.168.137.1:3000/admin/inquiry/view', {
          headers: {
            'Authorization': 'bearer ' + Vue.localStorage.get('token')
          }
        })
          .then((response) => {
            this.items = response.data
            console.log(response.data)
          })
          .catch((error) => {
            console.log(error)
          })
      } else {
        this.$router.push('/admin/login')
      }
    }
  }
</script>

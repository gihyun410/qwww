<template>
  <b-row>
    <b-col cols="12">
      <h2>단속 리스트
        <b-link @click="logout()">(Logout)</b-link>
      </h2>
      <b-table striped hover :items="books" :fields="fields">
        
      </b-table>
      <ul v-if="errors && errors.length">
        <li v-for="error of errors">
          <b-alert show>{{error.message}}</b-alert>
        </li>
      </ul>
    </b-col>
  </b-row>
</template>

<script>

import axios from 'axios'

export default {
  name: 'BookList',
  data () {
    return {
      fields: {
        isbn: { label: 'car number', sortable: true, 'class': 'text-center' },
        title: { label: 'data', sortable: true },
        actions: { label: 'locate', sortable: true, 'class': 'text-center' }
      },
      books: [],
      errors: []
    }
  },
  created () {
    axios.defaults.headers.common['Authorization'] = localStorage.getItem('jwtToken')
    axios.get(`http://localhost:3000/book`)
    .then(response => {
      this.books = response.data
    })
    .catch(e => {
      this.errors.push(e)
      if(e.response.status === 401) {
        this.$router.push({
          name: 'Login'
        })
      }
    })
  },
  methods: {
    logout () {
      localStorage.removeItem('jwtToken')
      this.$router.push({
        name: 'Login'
      })
    }
  }
}
</script>

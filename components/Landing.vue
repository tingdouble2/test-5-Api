<template>
  <v-container>
    <v-text-field v-model="msg" label="ข้อความ" />
    <v-row
      align="center"
      justify="space-around"
    >
      <div class="btn-submit">
        <v-btn type="submit" depressed @click="postData">
          SEND
        </v-btn>
      </div>
    </v-row>
    <v-simple-table class="table">
      <template #default>
        <thead>
          <tr>
            <th class="text-left">
              {{ title }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(items,index) in datas"
            :key="index"
          >
            <td>{{ items.msg }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
  </v-container>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Landing',
  data () {
    return {
      msg: '',
      title: 'ข้อความ',
      datas: []
    }
  },
  created () {
    this.fetchData()
  },
  methods: {
    async fetchData () {
      try {
        const url = await this.$axios.get('http://localhost:3001/getlist')
        this.datas = url.data.data
      } catch (err) {
        console.log('error')
      }
    },
    async postData () {
      try {
        const result = await axios.post('http://localhost:3001/postlist', {
          msg: this.msg
        })
        this.fetchData()
        this.msg = ''
        console.log(result)
      } catch (err) {
        console.log('error')
      }
    }
  }
}
</script>

<style>
ul {
  margin-top: 15px;
}

.table {
  margin-top: 15px;
}

.btn-submit{
  display: block;
  margin: auto;
}
</style>

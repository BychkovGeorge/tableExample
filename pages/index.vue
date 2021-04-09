<template>
  <main class="mw-100 mt-5 mb-5">
    <div class="row px-5">
      <Table class="w-100" :data="tableData" />
    </div>
  </main>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      tableData: null
    }
  },

  created () {
    axios.get('https://jsonplaceholder.typicode.com/albums').then((res) => {
      this.tableData = res.data
    })
  }
}
</script>

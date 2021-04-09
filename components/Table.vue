<template>
  <div class="col-12">
    <div class="row m-0 mb-3 justify-content-between">
      <div class="row m-0 align-items-center">
        <div class="mr-2">Show</div>
        <select @change.prevent="currentPage = 0" v-model="numberOfRows">
          <option>10</option>
          <option>15</option>
          <option>20</option>
        </select>
        <div class="ml-2">entries</div>
      </div>
      <div class="row m-0 align-items-center">
        <div class="mr-3">Search:</div>
        <input @keypress.enter="search" v-model="inputedValue" type="text">
        <div class="ml-3 pointer clear-link" v-if="searched" @click="clear">Clear</div>
      </div>
    </div>
    <main id="table" class="table-container w-100">
      <div class="row-line row m-0">
        <div class="col-3">
          <div class="ml-5">
            Action
          </div>
        </div>
        <div class="col-3">
          <div @click="sort('id')" class="pointer">
            Id
            <img v-if="sortedId !== ''" src="~/assets/img/arrow.svg" :class="{'rotated': sortedId === 'DESC'}" />
          </div>
        </div>
        <div class="col-3">
          <div @click="sort('title')" class="pointer">
            Title
            <img v-if="sortedTitle !== ''" src="~/assets/img/arrow.svg" :class="{'rotated': sortedTitle === 'DESC'}" />
          </div>
        </div>
        <div class="col-3">
          <div @click="sort('userId')" class="pointer">
            UserId
            <img v-if="sortedUserId !== ''" src="~/assets/img/arrow.svg" :class="{'rotated': sortedUserId === 'DESC'}" />
          </div>
        </div>
      </div>
      <TableRow
        :current="currentPage"
        :data="row"
        :index="i"
        :maxRows="numberOfRows"
        v-for="(row, i) in tableData"
        :key="row.id"
      />
    </main>
    <nav aria-label="Page navigation example">
      <ul v-if="tableData" class="pagination justify-content-center mt-5">
        <li @click="currentPage = ind" class="page-item pointer" v-for="(el, ind) in Math.ceil(tableData.length / numberOfRows)" :key="el">
          <a :class="{'selected': currentPage === ind}" class="page-link">
            {{ ind }}
          </a>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
export default {
  props: {
    data: {
      type: Array,
      required: true
    }
  },

  data () {
    return {
      numberOfRows: 10,
      inputedValue: '',
      sortedId: '',
      sortedTitle: '',
      sortedUserId: '',
      tableData: null,
      searched: false,
      filteredArray: [],
      currentPage: 0
    }
  },

  mounted () {
    setTimeout(() => {
      this.tableData = this.data
    }, 100)
  },

  methods: {
    sort (variant) {
      switch (variant) {
        case 'id': {
          this.sortedTitle = ''
          this.sortedUserId = ''
          if (this.sortedId === '') {
            this.sortedId = 'ASC'
            this.tableData.sort((a, b) => a.id - b.id)
          } else if (this.sortedId === 'ASC') {
            this.sortedId = 'DESC'
            this.tableData.sort((a, b) => b.id - a.id)
          } else if (this.sortedId === 'DESC') {
            this.sortedId = 'ASC'
            this.tableData.sort((a, b) => a.id - b.id)
          }
          break
        }
        case 'title': {
          this.sortedId = ''
          this.sortedUserId = ''
          if (this.sortedTitle === '') {
            this.sortedTitle = 'ASC'
            this.tableData.sort((a, b) => {
              if (a.title.toLowerCase() < b.title.toLowerCase()) {
                return -1
              } else if (a.title.toLowerCase() > b.title.toLowerCase()) {
                return 1
              }
              return 0
            })
          } else if (this.sortedTitle === 'ASC') {
            this.sortedTitle = 'DESC'
            this.tableData.sort((a, b) => {
              if (b.title.toLowerCase() < a.title.toLowerCase()) {
                return -1
              } else if (b.title.toLowerCase() > a.title.toLowerCase()) {
                return 1
              }
              return 0
            })
          } else if (this.sortedTitle === 'DESC') {
            this.sortedTitle = 'ASC'
            this.tableData.sort((a, b) => {
              if (a.title.toLowerCase() < b.title.toLowerCase()) {
                return -1
              } else if (a.title.toLowerCase() > b.title.toLowerCase()) {
                return 1
              }
              return 0
            })
          }
          break
        }
        case 'userId': {
          this.sortedTitle = ''
          this.sortedId = ''
          if (this.sortedUserId === '') {
            this.sortedUserId = 'ASC'
            this.tableData.sort((a, b) => a.id - b.id)
          } else if (this.sortedUserId === 'ASC') {
            this.sortedUserId = 'DESC'
            this.tableData.sort((a, b) => b.id - a.id)
          } else if (this.sortedUserId === 'DESC') {
            this.sortedUserId = 'ASC'
            this.tableData.sort((a, b) => a.id - b.id)
          }
          break
        }
      }
    },

    search () {
      this.searched = true
      this.filteredArray = this.tableData.filter((el) => {
        return !!el.title.includes(this.inputedValue)
      })
      this.tableData = this.filteredArray
      this.currentPage = 0
    },

    clear () {
      this.inputedValue = ''
      this.searched = false
      this.tableData = this.data
      this.currentPage = 0
    }
  }
}
</script>

<style scoped lang="sass">
.table-container
  box-shadow: 0px 2px 20px rgba(0, 74, 172, 0.15)
  border-radius: 6px

.row-line
  border-bottom: 1px solid grey
  height: 40px

.pointer
  cursor: pointer

.rotated
  transform: rotate(180deg)

.clear-link
  color: blue
  text-decoration: underline

.selected
  background: grey
</style>

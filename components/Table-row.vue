<template>
  <div>
    <div :id="data.id" class="row-line m-0 row w-100 align-items-center" v-if="index >= (current) * maxRows && index < (current + 1) * maxRows">
      <div class="pointer col-3">
        <button @click="colorize" class="btn btn-info">REFUSE</button>
        <a v-b-toggle :href="'#collapse' + data.id">
          <img
            v-if="!opened"
            @click.prevent="openInfo"
            class="ml-5"
            width="5%"
            src="~/assets/img/addition.svg"
          />
          <img
            v-if="opened"
            @click.prevent="closeInfo"
            class="ml-5"
            width="5%"
            src="~/assets/img/minus.svg"
          />
        </a>
      </div>
      <div class="col-3">
        {{ data.id }}
      </div>
      <div class="col-3">
        {{ data.title }}
      </div>
      <div class="col-3">
        {{ data.userId }}
      </div>
    </div>
    <div class="row m-0">
      <b-collapse class="col-12 p-0" :id="'collapse' + data.id">
        <b-card title="Info Image">
          <img v-if="info" height="40px" :src="info.url"/>
        </b-card>
      </b-collapse>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  props: {
    data: {
      type: Object,
      required: true
    },

    index: {
      type: Number,
      required: true
    },

    maxRows: {
      type: Number,
      required: true
    },

    current: {
      type: Number,
      required: true
    }
  },

  data () {
    return {
      opened: false,
      info: null
    }
  },

  methods: {
    openInfo () {
      this.opened = true
      axios.get(`https://jsonplaceholder.typicode.com/photos?id=${this.data.id}`).then((res) => {
        this.info = res.data[0]
      })
    },

    closeInfo () {
      this.opened = false
    },

    colorize () {
      document.getElementById(this.data.id).style.background = 'red'
    }
  }
}
</script>

<style scoped lang="sass">
.row-line
  min-height: 40px
  border-bottom: 1px solid grey

.pointer
  cursor: pointer
</style>

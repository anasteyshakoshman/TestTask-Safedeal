<template>
  <div id="app">
    <img
    v-for="obj in objects"
    :key="obj.id"
    :src="obj.url"
    @click="setUpShowModal(obj)"
    class="home__images"/>
    <modal
    v-if="showModal"
    :id="clickId"
    @close="setOutShowModal">
    </modal>
  </div>
</template>

<script>
import Modal from '@/components/Modal'
import axios from 'axios'
export default {
  name: 'App',
  components: {
    Modal
  },
  data: function () {
    return {
      objects: [],
      showModal: false,
      clickId: 0
    }
  },
  created () {
    axios
      .get('https://boiling-refuge-66454.herokuapp.com/images')
      .then((response) => {
        this.objects = response.data
      })
      .catch(error => console.log(error))
  },
  methods: {
    setUpShowModal (obj) {
      this.showModal = true
      this.clickId = obj.id
      console.log(this.clickId)
    },
    setOutShowModal () {
      this.showModal = false
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
img {
  width: 400px;
  margin: 10px;
}
</style>

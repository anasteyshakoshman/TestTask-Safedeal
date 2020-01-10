<template>
  <div id="app" class="app">
    <h1 class="app-title">TEST APP</h1>
    <img
    v-for="obj in objects"
    :key="obj.id"
    :src="obj.url"
    @click="setUpShowModal(obj)"
    class="app-img"/>
    <modal
    v-if="showModal"
    :object="extendObjects[clickNum]"
    @addComment="addNewComment($event)"
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
      extendObjects: [],
      clickNum: 0
    }
  },
  created () {
    axios
      .get('https://boiling-refuge-66454.herokuapp.com/images')
      .then((response) => {
        this.objects = response.data
        for (let i = 0; i < this.objects.length; i++) {
          this.objects[i].number = i
          this.getExtendObject(this.objects[i].id, i)
        }
      })
      .catch(error => console.log(error))
  },
  methods: {
    setUpShowModal (obj) {
      this.showModal = true
      this.clickNum = obj.number
    },
    setOutShowModal () {
      this.showModal = false
    },
    getExtendObject (id, num) {
      axios
        .get('https://boiling-refuge-66454.herokuapp.com/images/' + id)
        .then((response) => {
          this.extendObjects[num] = response.data
          this.extendObjects[num].number = num
          for (let i = 0; i <= this.extendObjects[num].comments.length; i++) {
            this.extendObjects[num].comments[i].date = this.convertDate(this.extendObjects[num].comments[i].date)
          }
        })
        .catch(error => console.log(error))
    },
    addNewComment (comment) {
      comment.date = this.convertDate(comment.date)
      this.extendObjects[this.clickNum].comments.push(comment)
    },
    convertDate (time) {
      let date = new Date(time)
      let mas = [date.getDate(), date.getMonth() + 1].map(function (el) {
        return el > 9 ? el : '0' + el
      })
      return mas[0] + '.' + mas[1] + '.' + date.getFullYear()
    }
  }
}
</script>

<style>
.app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}
.app-img {
  width: 400px;
  margin: 10px;
}

.app-title {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  font-weight: normal;
  font-size: 36px;
  line-height: 42px;
}
</style>

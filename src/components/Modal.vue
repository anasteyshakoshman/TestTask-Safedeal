<template>
  <transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">

          <div class="modal-header">
            <slot name="header">
              <h3>{{object.id}}</h3>
            </slot>
          </div>

          <div class="modal-body">
            <slot name="body">
              <img :src="object.url"/>
            </slot>
          </div>

          <div class="modal-footer">
            <slot name="footer">
              <div
              v-for="(comment, index) in comments"
              :key="index">
                <h3>{{comment.text}}</h3>
                <h3>{{comment.date}}</h3>
              </div>
              <button class="modal-default-button" @click="$emit('close')">
                OK
              </button>
            </slot>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>
<script>
import axios from 'axios'
export default {
  name: 'Modal',
  props: {
    id: {
      type: Number,
      required: true
    }
  },
  data: function () {
    return {
      object: null
    }
  },
  created () {
    axios
      .get('https://boiling-refuge-66454.herokuapp.com/images/' + this.id)
      .then((response) => {
        this.object = response.data
      })
      .catch(error => console.log(error))
  },
  computed: {
    url () {
      return this.object.url
    },
    comments () {
      return this.object.comments
    }
  }
}
</script>

<style scoped>
img {
  width: 200px;
}
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, .5);
  display: table;
  transition: opacity .3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 400px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
  transition: all .3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>

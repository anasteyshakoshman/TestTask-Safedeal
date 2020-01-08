<template>
  <transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="modal-info">
            <img
            class="modal-img"
            :src="object.url"/>
            <div
            v-for="(comment, index) in object.comments"
            :key="index">
              <h3 class="modal-date">{{comment.date}}</h3>
              <h3 :title="comment.name">{{comment.text}}</h3>
            </div>
            <a-input
            class="modal-input"
            v-model="nameComment"
            placeholder="Ваше имя" />
            <a-input
            class="modal-input"
            v-model="textComment"
            placeholder="Ваш комментарий" />
            <a-button
            type="primary"
            @click="sendNewComment">
              Оставить комментарий
            </a-button>
            <button class="modal-default-button" @click="$emit('close')"/>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>
<script>
export default {
  name: 'Modal',
  props: {
    object: {
      type: Object,
      required: true
    }
  },
  data: function () {
    return {
      nameComment: '',
      textComment: ''
    }
  },
  methods: {
    convertDate (date) {
      let day = date.getDate()
      if (day < 10) day = '0' + day
      let month = date.getMonth() + 1
      if (month < 10) month = '0' + month
      return day + '.' + month + '.' + date.getFullYear()
    },
    sendNewComment () {
      this.$emit('addComment', {
        name: this.nameComment,
        text: this.textComment,
        date: this.convertDate(new Date())
      })
      this.nameComment = ''
      this.textComment = ''
    }
  }
}
</script>

<style scoped>
.modal-img {
  width: 300px;
}
.modal-mask {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, .7);
  display: table;
  transition: opacity .3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 619px;
  height: 425px;
  margin: 0px auto;
  padding: 10px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
  transition: all .3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-default-button {
  float: right;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
.modal-input {
  width: 300px;
}
.modal-info {
  margin: 3px;
  padding: 0;
}
.modal-date {
  opacity: 0.6;
}
</style>

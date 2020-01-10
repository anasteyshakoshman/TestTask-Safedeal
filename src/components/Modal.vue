<template>
  <transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="modal-left">
            <img
            class="modal-img"
            :src="object.url"/>
            <a-input
            v-model="nameComment"
            placeholder="Ваше имя"
            class="modal-add-comment"/>
            <a-input
            v-model="textComment"
            placeholder="Ваш комментарий"
            class="modal-add-comment"/>
            <a-button
            type="primary"
            @click="sendNewComment"
            class="modal-add-comment">
              Оставить комментарий
            </a-button>
          </div>
          <div class="modal-middle">
            <div class="modal-comment"
             v-for="(comment, index) in object.comments"
             :key="index">
              <h3 class="modal-comment-date">{{comment.date}}</h3>
              <h3 :data-title="comment.name" class="modal-comment-text">{{comment.text}}</h3>
            </div>
          </div>
          <a-button
          icon="close"
          class="modal-default-button modal-right"
          @click="$emit('close')"/>
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
    sendNewComment () {
      if (!this.nameComment || !this.textComment) return
      this.$emit('addComment', {
        name: this.nameComment,
        text: this.textComment,
        date: Date.now()
      })
      this.nameComment = ''
      this.textComment = ''
    }
  }
}
</script>

<style scoped>
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
  width: 620px;
  height: 450px;
  margin: 0 auto;
  padding: 0 0 30px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
  transition: all .3s ease;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

.modal-left, .modal-middle, .modal-right {
  display: inline-block;
  vertical-align: top;
}

.modal-left, .modal-middle {
  margin-top: 30px;
  float: left;
}

.modal-left {
  width: 370px;
  margin-right: 15px;
}

.modal-middle {
  width: 160px;
  max-height: 388px;
  padding-right: 15px;
  overflow-y: scroll;
}

.modal-right {
  margin: 10px 10px 0 0;
}

.modal-add-comment {
  display: block;
  width: 100%;
}

.modal-img {
  display: block;
  max-height: 247px;
}

.modal-add-comment {
  margin-top: 18px;
}

.modal-comment {
  font-family: Helvetica, Arial, sans-serif;
  display: block;
  margin-bottom: 10px;
  font-size: 11px;
  text-align: left;
}

.modal-comment-date {
  margin: 0 auto;
  opacity: 0.6;
}

.modal-comment-text {
  overflow: hidden;
  text-overflow: ellipsis;
}

.modal-comment-text:hover::after {
  content: attr(data-title);
  z-index: 1;
  background: white;
  color: dodgerblue;
  font-family: Arial, sans-serif;
  font-size: 12px;
  padding: 1px 3px;
  margin-left: 10px;
  border: 1px solid dodgerblue;
  border-radius: 4px;
}

.modal-default-button {
  display: block;
  float: right;
}
</style>

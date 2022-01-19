<template>
  <div>
    <p class="text-muted">
        <strong>{{ answer.author }} - {{ answer.created_at }}</strong>
    </p>
    <p style="white-space: pre-wrap;">{{ answer.body }}</p>
    <div v-if="isAnswerAuthor">
      <router-link class="btn btn-sm btn-warning me-1"
        :to="{ name: 'answer-editor', params: {uuid: answer.uuid} }">
        Edit
      </router-link>
      <button class="btn btn-sm btn-danger mx-1"
        @click="showDeleteConfirmationBtn = !showDeleteConfirmationBtn">
        Delete
      </button>
      <button class="btn btn-sm btn-outline-danger mx-1"
        @click="triggerDeleteAnswer" v-show="showDeleteConfirmationBtn">
        Yes, delete my Answer!
      </button>
    </div>
    <div v-else>
      <button @click="toggleLike" class="btn"
        :class="{'btn-warning': userLikedAnswer, 'btn-outline-danger': !userLikedAnswer}">
        Like Answer
        <span class="badge bg-danger">{{ likesCounter }}</span>
      </button>
    </div>
    <hr>
  </div>
</template>

<script>
import axios from '@/common/api.service';

export default {
  name: 'AnswerComponent',
  props: {
    answer: {
      type: Object,
      required: true,
    },
    requestUser: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      showDeleteConfirmationBtn: false,
      userLikedAnswer: this.answer.user_has_liked_answer,
      likesCounter: this.answer.likes_count,
    };
  },
  computed: {
    isAnswerAuthor() {
      return this.answer.author === this.requestUser;
    },
  },
  methods: {
    triggerDeleteAnswer() {
      this.$emit('delete-answer', this.answer);
    },
    toggleLike() {
      if (this.userLikedAnswer) {
        this.unlikeAnswer();
      } else {
        this.likeAnswer();
      }
    },
    async likeAnswer() {
      this.userLikedAnswer = true;
      this.likesCounter += 1;
      const endpoint = `/api/v1/answers/${this.answer.uuid}/like/`;
      try {
        await axios.post(endpoint);
      } catch (error) {
        console.log(error.response);
      }
    },
    async unlikeAnswer() {
      this.userLikedAnswer = false;
      this.likesCounter -= 1;
      const endpoint = `/api/v1/answers/${this.answer.uuid}/like/`;
      try {
        await axios.delete(endpoint);
      } catch (error) {
        console.log(error.response);
      }
    },
  },
};
</script>

<style>

</style>

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
    </div>
    <hr>
  </div>
</template>

<script>
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
  computed: {
    isAnswerAuthor() {
      return this.answer.author === this.requestUser;
    },
  },
};
</script>

<style>

</style>

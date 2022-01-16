<template>
  <div class="mt-3">
    <div v-if="question">
      <h1>{{ question.content }}</h1>
      <p class="mb-0">
        Posted by:
        <span class="author-name">{{ question.author }}</span>
      </p>
      <p>{{ question.created_at }}</p>
    </div>
    <div v-else>
      <h1 class="error text-center">404 - Question Not Found</h1>
    </div>
  </div>
</template>

<script>
import axios from '@/common/api.service';

export default {
  name: 'Question',
  props: {
    slug: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      question: '',
    };
  },
  methods: {
    async getQuestionData() {
      const endpoint = `/api/v1/questions/${this.slug}/`;
      try {
        const response = await axios.get(endpoint);
        this.question = response.data;
        console.log(this.question);
      } catch (error) {
        console.log(error.response);
      }
    },
  },
  created() {
    this.getQuestionData();
  },
};
</script>

<style>
  .author-name {
    font-weight: bold;
    color: #dc35
  }
</style>

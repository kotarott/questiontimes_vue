<template>
  <div class="mt-3">
    <div v-if="question">
      <h1>{{ question.content }}</h1>
      <p class="mb-0">
        Posted by:
        <span class="author-name">{{ question.author }}</span>
      </p>
      <p>{{ question.created_at }}</p>

      <div v-if="userHasAnswered">
        <p class="answer-added">You've written an answer!</p>
      </div>
      <div v-else-if="showForm">
        <form @submit.prevent="onSubmit">
          <p>Answer the Question</p>
          <textarea rows="10" class="form-control"
            placeholder="Show your knowledge!"
            v-model="newAnswerBody"
            >
          </textarea>
          <button type="submit" class="btn btn-success my-3">
            Submit
          </button>
        </form>
        <p v-if="error" class="error mt-2">
          {{ error }}
        </p>
      </div>
      <div v-else>
        <button class="btn btn-success" @click="showForm = true">
          Answer the Question
        </button>
      </div>

      <hr>
    </div>

    <div v-else>
      <h1 class="error text-center">404 - Question Not Found</h1>
    </div>
    <div v-if="question" class="container">
      <answer-component
        v-for="answer in answers"
        :key="answer.uuid"
        :answer="answer">
      </answer-component>
    </div>
    <div class="my-4">
      <p v-show="loadingAnswers">...loading...</p>
      <button v-show="next" @click="getQuestionAnswers" class="btn btn-sm btn-outline-success m-2">
        lead More
      </button>
    </div>
  </div>
</template>

<script>
import axios from '@/common/api.service';
import AnswerComponent from '@/components/Answer.vue';

export default {
  name: 'Question',
  components: {
    AnswerComponent,
  },
  props: {
    slug: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      question: null,
      answers: [],
      next: null,
      loadingAnswers: false,
      userHasAnswered: false,
      showForm: false,
      newAnswerBody: null,
      error: null,
    };
  },
  methods: {
    async getQuestionData() {
      const endpoint = `/api/v1/questions/${this.slug}/`;
      try {
        const response = await axios.get(endpoint);
        this.question = response.data;
        this.userHasAnswered = response.data.user_has_answered;
        console.log(this.question);
        this.setPageTitle(response.data.content);
      } catch (error) {
        console.log(error.response);
        const title = '404 - Not Found.';
        this.setPageTitle(title);
      }
    },
    setPageTitle(title) {
      document.title = title;
    },
    async getQuestionAnswers() {
      let endpoint = `/api/v1/questions/${this.slug}/answers/`;
      if (this.next) {
        endpoint = this.next;
      }
      this.loadingAnswers = true;
      try {
        const response = await axios.get(endpoint);
        this.answers.push(...response.data.results);
        console.log(this.answers);
        this.loadingAnswers = false;
        if (response.data.next) {
          this.next = response.data.next;
        } else {
          this.next = null;
        }
      } catch (error) {
        console.log(error.response);
      }
    },
    async onSubmit() {
      if (!this.newAnswerBody) {
        this.error = "You can't send an empty answer!";
      }
      const endpoint = `/api/v1/questions/${this.slug}/answer/`;
      try {
        const response = await axios.post(endpoint, { body: this.newAnswerBody });
        this.answers.unshift(response.data);
        this.newAnswerBody = null;
        this.showForm = false;
        this.userHasAnswered = true;
        if (this.error) {
          this.error = null;
        }
      } catch (error) {
        console.log(error);
      }
    },
  },
  created() {
    this.getQuestionData();
    this.getQuestionAnswers();
  },
};
</script>

<style>
  .author-name {
    font-weight: bold;
    color: #dc35
  }

  .error {
    color: #dc3545;
  }

  .answer-added {
    font-weight: bold;
    color: green;
  }
</style>

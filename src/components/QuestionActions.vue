<template>
  <div class="mt-3">
    <router-link :to="{ name: 'question-editor', params: { slug: slug } }"
      class="btn btn-sm btn-warning me-1"
    >
      Edit
    </router-link>
    <button class="btn btn-sm btn-danger mx-1"
      @click="showDeleteConfirmationBtn = !showDeleteConfirmationBtn">
      Delete
    </button>
    <button class="btn btn-sm btn-outline-danger mx-1"
      @click="deleteQuestion" v-show="showDeleteConfirmationBtn">
      Yes, delete my Question!
    </button>
  </div>
</template>

<script>
import axios from '@/common/api.service';

export default {
  name: 'QuestionActions',
  props: {
    slug: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      showDeleteConfirmationBtn: false,
    };
  },
  methods: {
    async deleteQuestion() {
      const endpoint = `/api/v1/questions/${this.slug}/`;
      try {
        await axios.delete(endpoint);
        this.$router.push({ name: 'home' });
      } catch (error) {
        console.log(error.response);
      }
    },
  },
};
</script>

<style>

</style>

<template>
  <div id="nav">
    <navbar-component></navbar-component>
  </div>
  <router-view/>
</template>

<script>
import NavbarComponent from '@/components/Navbar.vue';
import axios from '@/common/api.service';

export default {
  name: 'App',
  components: {
    NavbarComponent,
  },
  methods: {
    async setUserInfo() {
      try {
        const response = await axios.get('/auth/users/me/');
        const requestUser = response.data.username;
        window.localStorage.setItem('username', requestUser);
      } catch (error) {
        console.log(error.response);
      }
    },
  },
  created() {
    this.setUserInfo();
  },
};
</script>

<style>
body {
  font-family: "Noto Sans JP", sans-serif;
  font-weight: 300;
}
</style>

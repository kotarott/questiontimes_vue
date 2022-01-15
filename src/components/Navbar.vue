<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light my-navbar">
  <div class="container-fluid">
    <!-- <a class="navbar-brand" href="#">QuestionTIme</a> -->
    <router-link class="navbar-brand" :to="{ name: 'home' }" >
        QuestionTIme
    </router-link>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="#">Home</a>
        </li>
      </ul>
      <a class="btn btn-outline-secondary" href="/accounts/logout/">Logout</a>
    </div>
  </div>
</nav>
</template>

<script>
export default {
  name: 'NavbarComponent',
};
</script>

<style>
.my-navbar {
  border-bottom: 1px solid #dddddd;
}

.navbar-brand {
  font-weight: bold;
  font-size: 150%;
}

.navbar-brand:hover {
  color: #dc3545 !important;
}
</style>

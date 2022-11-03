<template>
  <footer >
    <p>Copyright &copy; 2021</p>
    <router-link to="/about">About</router-link>
  </footer>
</template>

<script>
  export default {
    name: 'FooterVue',
  }
</script>

<style>
  a {
    color: #333;
  }

  footer {
    margin-top: 30px;
    text-align: center;
  }

</style>
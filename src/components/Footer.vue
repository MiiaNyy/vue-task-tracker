<template>
  <footer>
    <p>
      <em>Copyright &copy; 2022</em>
    </p>
    <router-link to="/about">About</router-link>
  </footer>
</template>

<script>
export default {
  name : "FooterMain"
}
</script>

<style scoped>
a {
  color: #333;
}

footer {
  margin-top: 30px;
  text-align: center;
  font-size: 0.7rem;
}
</style>

<template>
  <footer>
    <p>copyright &copy; 2021</p>
    <router-link to="/about"> About </router-link>
  </footer>
</template>
<script>
export default {
  name: "TaskFooter",
};
</script>

<style scoped>
a {
  color: aqua;
}
footer {
  margin-top: 30px;
  text-align: center;
}
</style>
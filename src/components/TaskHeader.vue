<template>
  <header>
    <h1>task tracker</h1>
    <AddButton
      v-show="homePage"
      @btn-clk="$emit('toggle-add-task')"
      :text="showAddTask ? 'Close' : 'Add task'"
      :color="showAddTask ? 'red' : 'green'"
    />
  </header>
</template>

<script>
import AddButton from "./AddButton";
export default {
  name: "TaskHeader",
  props: {
    title: String,
    showAddTask: Boolean,
  },
  components: {
    AddButton,
  },
  computed: {
    homePage() {
      if (this.$route.path === "/") {
        return true;
      } else {
        return false;
      }
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
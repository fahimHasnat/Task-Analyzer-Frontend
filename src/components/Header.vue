<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
      @btn-click="$emit('toggle-add-text')"
      :text="showAddText ? 'Close' : 'Add Text'"
      :color="showAddText ? 'red' : 'green'"
    />
  </header>
</template>

<script>
import Button from "./Button.vue";
export default {
  name: "Header",
  props: {
    title: String,
    showAddText: Boolean,
  },
  components: {
    Button,
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

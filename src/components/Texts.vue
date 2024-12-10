<template>
  <div :key="text.id" v-for="text in texts">
    <Text
      @update-text="$emit('update-text', $event)"
      @delete-text="$emit('delete-text', text.id)"
      :text="text"
    />
  </div>
</template>

<script>
import Text from "./Text";
export default {
  name: "Texts",
  props: {
    texts: Array,
  },
  components: {
    Text,
  },
  emits: ["delete-text", "update-text"],
};
</script>

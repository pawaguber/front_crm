<template>
  <v-alert
      type="props.status"
      title="props.text"
  ></v-alert>
</template>

<script setup>
const props = defineProps({
  status: String,
  text: String
});
</script>

<style scoped>

</style>
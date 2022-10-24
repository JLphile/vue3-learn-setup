<template>
  <HelloWorld></HelloWorld>
  <TheWelcome></TheWelcome>
</template>
<script>
import HelloWorld from "./components/HelloWorld.vue";
import TheWelcome from "./components/TheWelcome.vue";
export default {
  components: { HelloWorld, TheWelcome },
  data() {
    return {};
  },
};
</script>

<style scoped>
:deep(.title) {
  color: blue;
}
</style>

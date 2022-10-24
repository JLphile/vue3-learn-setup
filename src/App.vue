<template>
  <HelloWorld class="bar baz foo"></HelloWorld>
</template>
<script>
import HelloWorld from "./components/HelloWorld.vue";
export default {
  components: { HelloWorld },
  data() {
    return {
      msg: "Hello App",
      url: "https://cn.bing.com/",
      rawHtml: "This should be red.",
    };
  },
};
</script>

<style scoped>
.bar {
  color: red;
}
</style>

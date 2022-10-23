<template>
  <h2>插值语法</h2>
  <div>{{ msg }}</div>

  <hr />

  <h2>指令语法</h2>
  <HelloWorld></HelloWorld>
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

<style scoped></style>

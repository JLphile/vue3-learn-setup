<template>
  <div class="app-container">
    <h1>App根组件</h1>
  </div>
  <hr />

  <HeaderVue title="父组件传值" bgcolor="cyan" color="red"></HeaderVue>
</template>

<script>
import HeaderVue from "./HeaderVue.vue";
export default {
  components: {
    HeaderVue,
  },
  data() {
    return {};
  },
};
</script>

<style lang="less" scoped>
.app-container {
  margin-top: 45px;
}
</style>

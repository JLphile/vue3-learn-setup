<template>
  <div>
    <div class="header-box" :style="{ backgroundColor: bgcolor, color: color }">
      {{ title || "Header组件" }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: String,
    bgcolor: String,
    color: String,
  },
};
</script>

<style lang="less" scoped>
.header-box {
  height: 45px;
  background-color: pink;
  position: fixed;
  text-align: center;
  line-height: 45px;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 999;
}
</style>

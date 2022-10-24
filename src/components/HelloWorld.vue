<template>
  <p :class="{ active: isActive }">我是段落</p>
  <span :class="$attrs.class">span标签</span>
</template>

<script>
export default {
  data() {
    return {
      isActive: true,
      error: null,
    };
  },
  computed: {
    classObject() {
      return {
        active: this.isActive && !this.error,
        "text-danger": this.error && this.error.type === "fatal",
      };
    },
  },
};
</script>

<style scoped>
.bar {
  color: cyan;
}
</style>

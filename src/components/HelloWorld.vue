<template>
  <div :class="{ active: isActive }">active</div>
  <div class="static" :class="{ active: isActive, 'text-danger': hasError }">
    static active
  </div>
  <div :class="classObject"></div>
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

<style scoped></style>

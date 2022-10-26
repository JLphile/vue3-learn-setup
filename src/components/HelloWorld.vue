<template>
  <div>
    <h1>HelloWorld组件</h1>
    <h2 class="title">{{ fullName }}</h2>
    <button @click="changeFullName">changeFullName</button>
    <button @click="changeFirstName">changeFirstName</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      firstName: "hello",
      lastName: "world",
    };
  },
  computed: {
    fullName: {
      get: function () {
        console.log("get", this.fullName);

        return this.firstName + " " + this.lastName;
      },
      set: function (newValue) {
        console.log("set", newValue);
        const names = newValue.split(" ");
        this.firstName = names[0];
        this.lastName = names[1];
        console.log("set", this.fullName);
      },
    },
  },
  methods: {
    changeFullName() {
      this.fullName = "Coder Why";
    },
    changeFirstName() {
      this.firstName = "Tom";
    },
  },
};
</script>

<style scoped></style>

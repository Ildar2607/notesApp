<template>
  <div class="message">
    <p>Меssage component - {{message}}</p>
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      required: true,
      //default: "default message",
    },
  },
};
</script>

<style scoped>
.message {
  text-align: center;
  padding: 20px;
}
p {
  color: red;
}
</style>
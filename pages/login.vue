<script setup lang="ts">
definePageMeta({
  layout: "login",
})
</script>
<template>
  <div class="flex justify-center">
    <h1>Login</h1>
    <form class="flex flex-col">
      <input type="text" placeholder="Username" />
      <input type="password" placeholder="Password" />
      <button type="submit">Login</button>
    </form>
  </div>
</template>
<style></style>

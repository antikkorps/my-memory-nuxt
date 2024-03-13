<script setup lang="ts"></script>
<template>
  <nav>
    <ul>
      <li><router-link to="/">Home</router-link></li>
      <li><router-link to="/about">About</router-link></li>
      <li><router-link to="/my-memory">MyMemory</router-link></li>
    </ul>
  </nav>
</template>

<style>
nav {
  background-color: #333;
  color: white;
  padding: 1rem 2rem;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  display: inline;
  margin: 0 10px;
}
a {
  color: white;
  text-decoration: none;
}
</style>

<script setup>
import List from './components/UserList.vue';
import Form from './components/UserForm.vue';
</script>

<template>
  <main class="main">
    <Form />
    <List />
  </main>
</template>

<style lang="scss">
.main {
  display: flex;
  justify-content: center;
  gap: 16px;
}

.error {
    text-align: center;
    color: red;
    font-size: 20px;
  }
.success {
    text-align: center;
    color: green;
    font-size: 20px;
  }
</style>

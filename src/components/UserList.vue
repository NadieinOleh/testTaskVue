<script setup>
import {ref, onMounted} from 'vue';
import axios from 'axios';

const users = ref([]);
const error = ref('');

const getUsers = async () => {
  try {
    const response = await axios.get('https://reqres.in/api/users?page=1');
    users.value = response.data.data;
    console.log(response.data.data);
  } catch (err) {
    error.value = 'Oops, we have a problem';
  }
};

onMounted(() => {
  getUsers();
});
</script>

<template>
  <div>
    <ul class="list">
      <li v-for="user of users" :key="user.id" class="list__item">
        <img :src="user.avatar" :alt="user.email" />
        <p>{{ user.first_name }} {{ user.last_name }}</p>
        <p>{{ user.email }}</p>
      </li>
    </ul>
    <p class="error" v-if="error">{{ error }}</p>
  </div>
</template>

<style lang="scss">
.list {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 32px;
  flex-wrap: wrap;

  &__item {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
}
</style>

<script setup>
import {ref, onMounted, computed} from 'vue';
import axios from 'axios';
import UserDetail from './UserDetail.vue';

const users = ref([]);
const errorUser = ref('');
const selectedUser = ref(null);
const query = ref('');

const showUserDetails = user => {
  selectedUser.value = user;
};

const getUsers = async () => {
  try {
    const response = await axios.get('https://reqres.in/api/users?page=1');
    users.value = response.data.data;
    errorUser.value = '';
  } catch (err) {
    console.error(err);
    errorUser.value = 'Oops, we have a problem with user data';
  }
};
const removeUser = async id => {
  try {
    await axios.delete(`https://regres.in/api/users/${id}`);
    getUsers();
    console.log('remove success');
  } catch (err) {
    errorUser.value = 'Oops, we have a problem with removing user';
  }
};

const filteredUsers = computed(() => {
  if (!users.value || users.value.length === 0) return [];

  const lowerCaseSearch = query.value.toLowerCase();
  return users.value.filter(user => {
    const lowerCaseFirstName = (user.first_name || '').toLowerCase();
    const lowerCaseLastName = (user.last_name || '').toLowerCase();

    return lowerCaseFirstName.includes(lowerCaseSearch) || lowerCaseLastName.includes(lowerCaseSearch);
  });
});

onMounted(() => getUsers());
</script>

<template>
  <div>
    <div v-if="users.length !== 0"  class="box">
      <input v-model="query" type="text" placeholder="Search by name" class="search"/>

      <ul class="list">
        <li v-for="user of filteredUsers" :key="user.id" class="list__item">
          <img class="img" :src="user.avatar" :alt="user.email" />

          <p class="list__name" @click="showUserDetails(user)">{{ user.first_name }} {{ user.last_name }}</p>

          <p>{{ user.email }}</p>

          <button class="button" @click="removeUser(user.id)">Remove</button>
        </li>
      </ul>
    </div>
    <p class="error" v-else>{{ errorUser }}</p>
    <div class="error" v-if="query.length > 0 && filteredUsers.length === 0">
      {{ errorUser.value || 'No matches' }}
    </div>

    <UserDetail v-if="selectedUser" :user="selectedUser" @update:modelValue="selectedUser = null" />
  </div>
</template>

<style lang="scss">
.search {
  max-width: 200px;
  border-radius: 10px;
  padding: 5px;
  border: 0;
  margin: 20px;
  background-color: rgba(235, 235, 235, 0.64);
  font-size: 16px;
  
  &::placeholder {
    color: #ff0;
  }
}

.box {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

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

  &__name {
    cursor: pointer;
  }
}
</style>

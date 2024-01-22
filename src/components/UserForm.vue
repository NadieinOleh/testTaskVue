<script setup>
import axios from 'axios';
import {ref} from 'vue';

const firstName = ref('');
const lastName = ref('');
const email = ref('');
const error = ref('');
const success = ref('');

const validateEmail = email => {
  const emailRegex =
    /^(([^<>()\\[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
  return emailRegex.test(String(email).toLowerCase());
};

const addUser = async () => {
  if (!firstName.value || !lastName.value || !validateEmail(email.value)) {
    error.value = 'Please enter valid data';
    return;
  }


  try {
    await axios.post('https://reqres.in/api/users', {
      first_name: firstName.value,
      last_name: lastName.value,
      email: email.value,
    });

    firstName.value = '';
    lastName.value = '';
    email.value = '';
    error.value = '';
    success.value = 'We added the new user';

    setTimeout(() => {
      success.value = '';
    }, 1000)
  } catch (err) {
    error.value = "Oops, we can't add the new user";
  }
};

const handleSubmit = () => {
  addUser();
};
</script>

<template>
  <form class="form" @submit.prevent="handleSubmit">
    <h1>Add a user</h1>

    <label for="firstName">First name</label>
    <input class="form__input" id="firstName" type="text" v-model="firstName" placeholder="Enter a first name" />

    <label for="lastName">Last name</label>
    <input class="form__input" id="lastName" type="text" v-model="lastName" placeholder="Enter a last name" />

    <label for="email">Email</label>
    <input class="form__input" id="email" type="email" v-model="email" placeholder="Enter a email" />

    <button class="button" type="submit">Add User</button>

    <p v-show="error" class="error">{{ error }}</p>
    <p v-show="success && !error" class="success">{{ success }}</p>
  </form>
</template>

<style lang="scss">
.form {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  margin: 10px 0;

  &__input {
    max-width: 300px;
    min-width: 200px;
    border-radius: 10px;
    background-color: rgba(235, 235, 235, 0.64);
    padding: 4px 8px;
    border: 0;

    color: #0e312c;
    font-size: 18px;

    &::placeholder {
      color: #fff;
    }
  }
}
</style>

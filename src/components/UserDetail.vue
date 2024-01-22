<script setup>
import {ref} from 'vue';

const tel = ref('')
const address = ref('')
const {user} = defineProps(['user']);
const emit = defineEmits(['update:modelValue']);
</script>

<template>
  <div class="modal" @update:model-value="val => emit('update:modelValue', val)">
    <div class="modal__box">
      <h1 class="modal__text">
        {{ user.first_name }} {{ user.last_name }}
      </h1>

      <p class="modal__text">{{ user.email }}</p>

      <img class="img" :src="user.avatar" :alt="user.email"/>

      <h3 class="modal__text">Additional information</h3>

      <label  class="modal__text" for="tel">Phone</label>
      <input v-model="tel" type="tel" id="tel" class="modal__addInfo"/>

      <label class="modal__text" for="address">Address</label>
      <input v-model="address" id="address" class="modal__addInfo" />

      <button class="button" @click="emit('update:modelValue', false)">Back</button>
    </div>
  </div>
</template>

<style lang="scss">
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.9);
  z-index: 999;

  &__box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 8px;
    padding: 20px;
    background-color: #fff;
    border: 1px solid #ff9143;
    border-radius: 10px;
  }

  &__text {
    color: black;
    font-weight: 600;
  }

  &__addInfo {
    border-radius: 5px;
    background-color: rgba(235, 235, 235, 0.64);
    border: 1px solid #ff9143;
    padding: 4px;
    font-size: 18px;
  }
}
</style>

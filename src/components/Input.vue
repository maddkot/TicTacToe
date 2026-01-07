<script setup lang="ts">
import { reactive, toRaw } from "vue";

const options = [
  { label: "первый", value: "first" },
  { label: "второй", value: "second" },
  { label: "третий", value: "third" },
];
const form = reactive({
  name: "",
  phone: null,
  chekcbox: false,
  selectinput: [],
});
const sendConsoleForm = () => {
  console.log("toRaw:", toRaw(form));
};
</script>

<template>
  <div class="container">
    <input v-model="form.name" />
    <input type="number" v-model="form.phone" />
    <input type="checkbox" v-model="form.chekcbox" />
    <select v-model="form.selectinput">
      <option v-for="option in options" :key="option.value" :value="option">
        {{ option.label }}
      </option>
    </select>
    <button type="button" @click="sendConsoleForm">Ввести</button>
  </div>
</template>

<style lang="css" scoped>
.container {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
</style>

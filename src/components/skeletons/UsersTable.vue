<template>
  <div class="card mt-4 mb-4">
    <div class="card-body px-0 pt-0 pb-2">
      <div class="table-responsive p-0">
        <table class="table align-items-center mb-0">
          <thead>
            <tr>
              <th class="text-secondary text-xxs font-weight-bolder opacity-7">
                Avatar
              </th>
              <th
                class="text-start text-uppercase text-secondary text-xxs font-weight-bolder opacity-7"
              >
                First Name
              </th>
              <th
                class="text-start text-uppercase text-secondary text-xxs font-weight-bolder opacity-7"
              >
                Last Name
              </th>
              <th
                class="text-start text-uppercase text-secondary text-xxs font-weight-bolder opacity-7"
              >
                Email
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in users" :key="user">
              <td>
                <div class="d-flex px-2 py-1">
                  <div class="avatar"></div>
                </div>
              </td>
              <td class="align-middle text-center">
                <div class="text"></div>
              </td>
              <td class="align-middle text-center text-sm">
                <div class="text"></div>
              </td>
              <td class="align-middle text-center text-sm">
                <div class="text"></div>
              </td>
              <td class="align-middle text-center">
                <div class="actions"></div>
              </td>
              <td class="align-middle">
                <div class="actions"></div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted } from "vue";
import VsudAvatar from "@/components/VsudAvatar.vue";

const users = Array(8);
</script>

<style scoped>
.text {
  width: 120px;
  height: 10px;
  border-radius: 5px;
  animation: pulse-bg 1s infinite;
}
.actions {
  width: 50px;
  height: 10px;
  border-radius: 5px;
  animation: pulse-bg 1s infinite;
}
.avatar {
  width: 30px;
  height: 30px;
  border-radius: 30%;
  animation: pulse-bg 1s infinite;
}

@keyframes pulse-bg {
  0% {
    background-color: #eee;
  }
  50% {
    background-color: #c9c9c9;
  }
  100% {
    background-color: #eee;
  }
}
</style>

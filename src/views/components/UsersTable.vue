<template>
  <a class="add" @click="addUser"> + add user</a>

  <div class="card mt-4 mb-4">
    <div class="card-body px-0 pt-0 pb-2">
      <div class="table-responsive p-0">
        <table class="table align-items-center mb-0 p-2">
          <thead>
            <tr>
              #
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
            <tr v-for="user in users" :key="user.id">
              <td>
                {{user.id}}
                <div class="d-flex px-2 py-1">
                  <vsud-avatar
                    :img="user.avatar"
                    size="sm"
                    border-radius="lg"
                    class="me-3"
                    alt="user1"
                  />
                </div>
              </td>
              <td class="align-middle text-center text-sm">
                {{ user.first_name }}
              </td>
              <td class="align-middle text-center text-sm">
                {{ user.last_name }}
              </td>
              <td class="align-middle text-center text-sm">
                {{ user.email }}
              </td>
              <td class="align-middle text-center">
                <span
                  @click="deleteUser(user.id)"
                  class="btn btn-outline-danger btn-sm text-xs font-weight-bold"
                >
                  delete</span
                >
              </td>
              <td class="align-middle">
                <a
                  href="javascript:;"
                  @click="editUser(user.id)"
                  class="btn btn-outline-secondary btn-sm"
                  data-toggle="tooltip"
                  data-original-title="Edit user"
                  >Edit</a
                >
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <Pagination :pages="total_pages" @navigateToPage="navigateToPage" />
  </div>
</template>

<script setup>
import { onMounted, ref, } from "vue";
import axios from "axios";
import VsudAvatar from "@/components/VsudAvatar.vue";
import Pagination from "@/components/Pagination.vue";
import Swal from "sweetalert2";
import store from "../../store/index"
let users = ref([]);
let total_pages = ref(1);

const getUsers = async (currentPage = 1, limit = 10) => {
  const res = await axios.get(
    `${store.state.endpoint}/users?page=${currentPage}&limit=${limit}`
  );
  users.value = res.data.data;
  total_pages.value = res.data?.total_pages;
};

const addUser = async () => {
  Swal.showLoading();
  const res = await axios.post(`${store.state.endpoint}/users`);
  Swal.hideLoading();
  const added = res.data.data.status;
  if (added) {
    new Swal({
      title: "User has been added succesfuly",
      icon: "success",
      confirmButtonColor: "#41b882",
    });
    users.value = getUsers();
  }
};

const deleteUser = async (id) => {
  Swal.fire({
    title: "Are you sure?",
    text: "You won't be able to revert this!",
    icon: "warning",
    showCancelButton: true,
    cancelButtonColor: "#3085d6",
    confirmButtonColor: "#d33",
    confirmButtonText: "Yes, delete it!",
  }).then(async (result) => {
    if (result.isConfirmed) {
      const res = await axios.delete(`${store.state.endpoint}/users/${id}`);
      const deleted = res.data.data.status;
      if (deleted) {
        new Swal({
          title: "User has been deleted succesfuly",
          icon: "success",
          timer: 2000,
          postion:'top',
          toast: true,
          confirmButtonColor: "#41b882",
        });
        users.value = getUsers();
      }
    }
  });
};

const editUser = async (id) => {
  Swal.showLoading();
  const res = await axios.put(`${store.state.endpoint}/users/${id}`);
  Swal.hideLoading();
  const updated = res.data.data.status;
  if (updated) {
    new Swal({
      title: "User has been updated succesfuly",
      icon: "success",
      confirmButtonColor: "#41b882",
    });
    users.value = getUsers();
  }
};

const navigateToPage = async (page) => {
  await getUsers(page);
};
await getUsers();
</script>

<style>
.add {
  position: relative;
  right: 0;
  margin-block: 1rem;
  padding: 0.6rem 0.8rem;
  color: wheat;
  background-color: rgb(37, 179, 32);
  border-radius: 5px;
  font-size: 1rem;
  box-shadow: 0 2px 12px 0 rgb(0 0 0 / 16%);
  cursor: pointer;
}
.swal2-container{
  z-index: 999999;
}
</style>

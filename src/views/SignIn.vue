<template>
  <main class="mt-0 main-content main-content-bg">
    <section>
      <div class="page-header min-vh-75">
        <div class="container">
          <div class="row">
            <div class="mx-auto col-xl-4 col-lg-5 col-md-6 d-flex flex-column">
              <div class="mt-8 card card-plain">
                <div class="pb-0 card-header text-start">
                  <h3 class="font-weight-bolder text-info text-gradient">
                    Welcome back
                  </h3>
                  <p class="mb-0">Enter your email and password to sign in</p>
                </div>
                <div class="card-body">
                  <form role="form" class="text-start" @submit.prevent="login">
                    <label>Email</label>
                    <div class="form-group">
                      <input
                        type="email"
                        class="form-control"
                        name="email"
                        placeholder="email"
                        v-model="email"
                        required
                      />
                    </div>
                    <label>Password</label>
                    <vsud-input
                      type="password"
                      placeholder="Password"
                      name="password"
                    />
                    <vsud-switch id="rememberMe" checked
                      >Remember me</vsud-switch
                    >
                    <div class="text-center">
                      <vsud-button
                        class="my-4 mb-2"
                        variant="gradient"
                        color="info"
                        full-width
                        type="submit"
                        >Sign in</vsud-button
                      >
                    </div>
                  </form>
                </div>
              </div>
            </div>
            <div class="col-md-6">
              <div
                class="top-0 oblique position-absolute h-100 d-md-block d-none me-n8"
              >
                <div
                  class="bg-cover oblique-image position-absolute fixed-top ms-auto h-100 z-index-0 ms-n6"
                  :style="{
                    backgroundImage: `url(${bgImg})`,
                  }"
                ></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script setup>
import { onBeforeMount, onBeforeUnmount, ref } from "vue";
import { useStore } from "vuex";
import { useRouter } from "vue-router";
import VsudInput from "@/components/VsudInput.vue";
import VsudSwitch from "@/components/VsudSwitch.vue";
import VsudButton from "@/components/VsudButton.vue";
import bgImg from "@/assets/img/curved-images/curved9.jpg";
import axios from "axios";
import Swal from "sweetalert2";
// import store from "../store/index";

const body = document.getElementsByTagName("body")[0];
let email = ref("");
const router = useRouter();
const store = useStore();
const login = async () => {
  Swal.showLoading();
  const res = await axios.post(`${store.state.endpoint}/auth/login`, {
    email: email.value,
  });

  const { user, status, error } = res.data.data;
  Swal.hideLoading();
  if (status == false) {
    Swal.fire({
      text: error,
      icon: "warning",
    });
  } else {
    store.commit("logUserIn", user);
    router.push({ name: "Profile" });
  }
};

onBeforeMount(() => {
  store.state.hideConfigButton = true;
  store.state.showNavbar = false;
  store.state.showSidenav = false;
  store.state.showFooter = false;
  body.classList.remove("bg-gray-100");
});

onBeforeUnmount(() => {
  store.state.hideConfigButton = false;
  store.state.showNavbar = true;
  store.state.showSidenav = true;
  store.state.showFooter = true;
  body.classList.add("bg-gray-100");
});
</script>

<script>
import { mapActions } from "pinia";
import { useAdminStore } from "../stores/admin";
import Swal from 'sweetalert2'

export default {
  data() {
    return {
      formLogin: {
        email: "",
        password: "",
      },
    };
  },
  methods: {
    ...mapActions(useAdminStore, ["login",'googleSignIn']),
    async loginHandler() {
      try {
        const { data } = await this.login(this.formLogin);
        localStorage.setItem('access_token', data.access_token)
        this.$router.push('/admin')
      } catch (err) {
        
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: err.response.data.msg,
        });
      }
    },
    async googleHandler() {
      try {
        const { data } = await this.googleSignIn();
        localStorage.setItem("access_token", data.access_token);

        Swal.fire({
          icon: "success",
          title: "Congrats!",
          timer: 2500
        });

        this.$router.push("/admin");
      } catch ({ response }) {
        console.log(response)
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: response.data.msg,
        });
      }
    },
  },
};
</script>

<template>
  <div class="d-flex align-items-center justify-content-center content-admin">
    <div class="row-colomn" style="height: 100%">
      <div class="text-center mb-3 text-uppercase">
        <h1 class="h1">admin</h1>
      </div>
      <div class="card shadow bg-linier-1">
        <div class="row mb-5 mt-2 justify-content-center">
          <div class="col-12 mb-5 text-center">
            <h3 class="h3">Login</h3>
            <p class="h6">
              If you have a acount you can sign in here, or you can sign in as a
              staff by login with google account
            </p>
          </div>
          <div class="col-10">
            <form id="login-form" @submit.prevent="loginHandler">
              <div class="form-floating mb-3">
                <input
                  v-model="formLogin.email"
                  type="email"
                  class="form-control"
                  id="login-email"
                  placeholder="email"
                />
                <label for="floatingInput">Email</label>
              </div>
              <div class="form-floating mb-3">
                <input
                  v-model="formLogin.password"
                  type="password"
                  class="form-control"
                  id="login-password"
                  placeholder="password"
                />
                <label for="floatingInput">Password</label>
              </div>
              <div class="row justify-content-center mt-5">
                <button
                  class="btn btn-lg btn-outline-primary rounded-pill w-100 p-2 mt-3 text-light"
                  type="submit"
                  id="btn-login-form"
                >
                  Login
                </button>
                <h4 class="text-center text-light p-2 mt-4">----- OR -----</h4>
                <button
                  class="btn btn-lg btn-outline-primary rounded-pill w-100 p-2 mt-2 text-light"
                  type="button"
                  @click.prevent="googleHandler"
                >
                  Login With Google
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

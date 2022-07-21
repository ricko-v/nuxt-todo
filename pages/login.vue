<template>
  <div class="d-flex justify-content-center">
    <div class="col-12 col-md-12 col-lg-4 py-3 card shadow-sm min-vh-100">
      <div class="mb-5 d-flex justify-content-between">
        <div>
          <NuxtLink to="/" class="text-dark">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="16"
              height="16"
              fill="currentColor"
              class="bi bi-arrow-left"
              viewBox="0 0 16 16"
            >
              <path
                fill-rule="evenodd"
                d="M15 8a.5.5 0 0 0-.5-.5H2.707l3.147-3.146a.5.5 0 1 0-.708-.708l-4 4a.5.5 0 0 0 0 .708l4 4a.5.5 0 0 0 .708-.708L2.707 8.5H14.5A.5.5 0 0 0 15 8z"
              />
            </svg>
          </NuxtLink>
        </div>
        <div class="d-flex align-items-center">
          <img class="img-icon" src="/icon.png" />
          <span>Nuxt-Todo</span>
        </div>
      </div>
      <h3 class="text-center text-danger font-weight-bold">Login User</h3>
      <hr />
      <div>
        <label for="username">Username:</label>
        <input
          type="text"
          v-model="username"
          id="username"
          class="form-control shadow-none mb-3"
        />
        <label for="password">Password:</label>
        <input
          :type="lihatpw == 'tidak' ? 'password' : 'text'"
          v-model="password"
          id="pasword"
          class="form-control shadow-none"
        />
        <b-form-checkbox
          id="checkbox-1"
          v-model="lihatpw"
          name="checkbox-1"
          value="ya"
          class="mb-5"
          unchecked-value="tidak"
        >
          <small><small>lihat password</small></small>
        </b-form-checkbox>

        <button
          v-if="load"
          class="btn btn-success rounded-pill w-100 shadow-none"
        >
          <b-spinner label="Spinning" small></b-spinner>
        </button>
        <button
          @click="login"
          v-if="!load"
          class="btn btn-success rounded-pill w-100 shadow-none"
        >
          <span>Masuk</span>
        </button>

        <p class="text-center mt-3">
          <small class="text-secondary"
            >Belum mempunyai akun?
            <NuxtLink to="/register">Buat Akun</NuxtLink></small
          >
        </p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      lihatpw: "tidak",
      username: "",
      password: "",
      load: true,
    };
  },
  mounted() {
    if (!localStorage.NT_LOGIN) {
      this.load = false;
    } else {
      this.$router.push({
        path: "/task",
      });
    }
  },
  methods: {
    login() {
      if (!localStorage.NT_AKUN) {
        this.$bvToast.toast(`Username atau Password salah!`, {
          title: `Pesan`,
          variant: "danger",
          toaster: "b-toaster-top-center",
          solid: true,
          appendToast: true,
        });
      } else {
        let akun = JSON.parse(localStorage.NT_AKUN);
        let cari = akun.find(
          (x) => x.username == this.username && x.password == this.password
        );

        if (cari && akun) {
          localStorage.NT_LOGIN = JSON.stringify({
            nama: cari.nama,
            username: cari.username,
          });
          this.$router.push({
            path: "/task",
          });
        } else {
          this.$bvToast.toast(`Username atau Password salah!`, {
            title: `Pesan`,
            variant: "danger",
            toaster: "b-toaster-top-center",
            solid: true,
            appendToast: true,
          });
        }
      }
    },
  },
};
</script>

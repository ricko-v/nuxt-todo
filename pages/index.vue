<template>
  <div class="d-flex justify-content-center">
    <div class="col-12 col-md-12 col-lg-4 py-3 card shadow-sm min-vh-100">
      <div class="d-flex align-items-center mb-5">
        <img class="img-icon" src="/icon.png" />
        <span>Nuxt-Todo</span>
      </div>
      <h5 class="text-center font-weight-bold">
        Atur rencanamu dengan <br />
        menggunakan <span class="text-danger"><u>Nuxt-Todo</u></span
        >.
      </h5>
      <div class="d-flex justify-content-center">
        <img src="/hero.svg" class="img-fluid" />
      </div>
      <small class="text-secondary text-center"
        >Ketika kamu mempunyai banyak <br />
        sekali tugas yang harus dikerjakan.</small
      >
      <div class="mt-4 d-flex justify-content-center">
        <button
          v-if="load"
          class="btn btn-success rounded-pill w-50 shadow-none"
        >
          <b-spinner label="Spinning" small></b-spinner>
        </button>
        <NuxtLink v-if="!load" to="/login" class="w-100 text-center">
          <button class="btn btn-success rounded-pill w-50 shadow-none">
            <span v-if="!load">Buat Sekarang</span>
          </button>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      status: "",
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
};
</script>

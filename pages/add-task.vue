<template>
  <div class="d-flex justify-content-center">
    <div class="col-12 col-md-12 col-lg-4 py-3 card shadow-sm min-vh-100">
      <div class="mb-5 d-flex justify-content-between">
        <div>
          <NuxtLink to="/task" class="text-dark">
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
      <h3 class="text-center text-danger font-weight-bold">Tambah Tugas</h3>
      <hr />
      <div>
        <label for="username">Nama Tugas:</label>
        <input
          type="text"
          v-model="judul"
          id="judul"
          class="form-control shadow-none mb-3"
        />
        <label for="deskripsi">deskripsi:</label>
        <textarea
          type="text"
          v-model="deskripsi"
          id="deskripsi"
          class="form-control shadow-none mb-5"
        />

        <button
          v-if="load"
          class="btn btn-success rounded-pill w-100 shadow-none"
        >
          <b-spinner label="Spinning" small></b-spinner>
        </button>
        <button
          @click="tambah"
          v-if="!load"
          class="btn btn-success rounded-pill w-100 shadow-none"
        >
          <span>Tambahkan</span>
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      load: true,
      judul: "",
      deskripsi: "",
    };
  },
  mounted() {
    if (!localStorage.NT_LOGIN) {
      this.$router.push({
        path: "/login",
      });
    } else {
      this.load = false;
    }
  },
  methods: {
    tambah() {
      if (this.judul == "" || this.deskripsi == "") {
        this.$bvToast.toast(`Data masih kosong!`, {
          title: `Pesan`,
          variant: "danger",
          toaster: "b-toaster-top-center",
          solid: true,
          appendToast: true,
        });
      } else {
        let post;

        if (!localStorage.NT_TASK) {
          post = [
            {
              judul: this.judul,
              deskripsi: this.deskripsi,
              status: "belum",
              username: JSON.parse(localStorage.NT_LOGIN)["username"],
              id: Math.floor(Math.random() * 1000000),
            },
          ];
          localStorage.NT_TASK = JSON.stringify(post);
        } else {
          post = {
            judul: this.judul,
            deskripsi: this.deskripsi,
            status: "belum",
            username: JSON.parse(localStorage.NT_LOGIN)["username"],
            id: Math.floor(Math.random() * 1000000),
          };
          let getPost = JSON.parse(localStorage.NT_TASK);
          getPost.push(post);
          localStorage.NT_TASK = JSON.stringify(getPost);
        }

        this.judul = "";
        this.deskripsi = "";

        this.$router.push({
          path: "/task",
        });
      }
    },
  },
};
</script>

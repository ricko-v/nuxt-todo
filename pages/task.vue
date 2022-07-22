<template>
  <div class="d-flex justify-content-center">
    <div class="col-12 col-md-12 col-lg-4 py-3 card shadow-sm min-vh-100">
      <div class="d-flex justify-content-between align-items-center">
        <div>
          <h4 class="font-weight-bold m-0">{{ nama.nama }}</h4>
          <small class="text-secondary">@{{ nama.username }}</small>
        </div>
        <div class="d-flex align-items-center">
          <button @click="logout" class="btn btn-outline-danger shadow-none">
            Logout
          </button>
        </div>
      </div>
      <hr />

      <div class="my-4">
        <b-input-group>
          <b-form-input
            v-model="q"
            :disabled="isCari ? true : false"
            class="shadow-none"
            placeholder="cari task..."
          ></b-form-input>
          <b-input-group-append>
            <b-button
              @click="cari"
              v-if="!isCari"
              class="shadow-none"
              variant="dark"
              >Cari</b-button
            >
            <b-button
              @click="kosongkan"
              v-if="isCari"
              class="shadow-none"
              variant="dark"
              >X</b-button
            >
          </b-input-group-append>
        </b-input-group>
      </div>

      <div>
        <div class="d-flex mb-4 justify-content-between align-items-center">
          <h5 class="font-weight-bold m-0">Daftar Tugas</h5>
          <NuxtLink class="text-success" to="/add-task">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              fill="currentColor"
              class="bi bi-plus-circle"
              viewBox="0 0 16 16"
            >
              <path
                d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"
              />
              <path
                d="M8 4a.5.5 0 0 1 .5.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3A.5.5 0 0 1 8 4z"
              />
            </svg>
          </NuxtLink>
        </div>
        <hr />
        <div
          v-if="
            arrTugas.length == 0 ||
            arrTugas.filter((x) => x.username == nama.username).length <= 0
          "
        >
          <h6 class="text-center text-secondary font-weight-bold">
            Belum ada tugas
          </h6>
        </div>

        <div v-for="task in arrTugas">
          <div
            :class="task.status == 'selesai' ? 'bg-success text-white' : ''"
            class="card p-3 mb-3"
            v-if="arrTugas && task.username == nama.username"
          >
            <div class="d-flex">
              <div class="mr-2">
                <input
                  v-if="task.status == 'selesai'"
                  type="checkbox"
                  @click="unceklist(task.id)"
                  checked
                />
                <input
                  v-if="task.status == 'belum'"
                  type="checkbox"
                  @click="ceklist(task.id)"
                />
              </div>
              <div
                class="w-100 d-flex justify-content-between align-items-center"
              >
                <div>
                  <b>{{ task.judul }}</b>
                  <p class="m-0">
                    <small
                      ><small>{{ task.deskripsi }}</small></small
                    >
                  </p>
                </div>
                <div>
                  <button
                    @click="hapus(task.id)"
                    :class="
                      task.status == 'selesai'
                        ? 'btn-danger'
                        : 'btn-outline-danger'
                    "
                    class="btn shadow-none"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      width="16"
                      height="16"
                      fill="currentColor"
                      class="bi bi-trash"
                      viewBox="0 0 16 16"
                    >
                      <path
                        d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"
                      />
                      <path
                        fill-rule="evenodd"
                        d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"
                      />
                    </svg>
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      nama: "",
      arrTugas: [],
      isCari: false,
      q: "",
    };
  },
  mounted() {
    if (!localStorage.NT_LOGIN) {
      this.$router.push({
        path: "/login",
      });
    } else {
      this.nama = JSON.parse(localStorage.NT_LOGIN);
      this.getTask();
    }
  },
  methods: {
    logout() {
      localStorage.removeItem("NT_LOGIN");
      this.$router.push({
        path: "/login",
      });
    },
    kosongkan() {
      this.q = "";
      this.getTask();
      this.isCari = false;
    },
    cari() {
      let tugas = this.arrTugas;
      let cari = tugas.find((x) => x["judul"].search(this.q) != -1);
      if (cari) {
        this.arrTugas = [
          {
            judul: cari.judul,
            deskripsi: cari.deskripsi,
            status: cari.status,
            username: cari.username,
          },
        ];
      } else {
        this.$bvToast.toast(`Tugas tidak ada!`, {
          title: `Pesan`,
          variant: "danger",
          toaster: "b-toaster-top-center",
          solid: true,
          autoHideDelay: 1000,
          appendToast: true,
        });
      }

      this.isCari = true;
    },
    getTask() {
      if (localStorage.NT_TASK) {
        this.arrTugas = "";
        this.arrTugas = JSON.parse(localStorage.NT_TASK).reverse();
      }
    },
    ceklist(id) {
      let tugas = this.arrTugas;
      tugas.find((x) => {
        if (x.id == id) {
          x.status = "selesai";
        }
      });

      localStorage.NT_TASK = JSON.stringify(tugas);
    },
    unceklist(id) {
      let tugas = this.arrTugas;
      tugas.find((x) => {
        if (x.id == id) {
          x.status = "belum";
        }
      });
      localStorage.NT_TASK = JSON.stringify(tugas);
    },
    hapus(id) {
      let tugas = this.arrTugas;
      let filgas = tugas.filter((x) => x.id != id);
      localStorage.NT_TASK = JSON.stringify(filgas);
      this.arrTugas = [{}];

      setTimeout(this.getTask, 100);
    },
  },
};
</script>

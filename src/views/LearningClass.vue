<template>
  <div class="container mt-5">
    <div class="about" v-for="learnings in learning" :key="learnings.id">
      <div class="aboutLearnings mb-5">
        <h1>Kelas {{ learnings.name }}</h1>
        <h2>{{ learnings.description }}</h2>
      </div>
      <div class="learnings row">
        <div class="col-md-6" v-for="learn in learnings.mentors.slice(0)" :key="learn">
          <h3>Mentor: {{ learn.name }}</h3>
          <h3>Pekerjaan: {{ learn.description }}</h3>
        </div>
        <div class="col-md-6"></div>
      </div>
    </div>
    <div class="d-grid gap-2 mt-3">
      <button
        type="button"
        class="btn btn-success"
        data-bs-toggle="modal"
        data-bs-target="#exampleModal"
      >
        Daftar Kelas Sekarang Juga
      </button>
    </div>

    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
      ref="modal"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-body">
            <form @submit.stop.prevent>
              <div v-if="errors.length">
                <b>Terjadi error berikut:</b>
                <ul>
                  <li v-for="error in errors" :key="error">{{ error }}</li>
                </ul>
              </div>
              <div class="mb-3 form-group">
                <label for="formFullName" class="form-label"
                  >Nama Lengkap
                  <input
                    type="text"
                    class="form-control"
                    id="formFullName"
                    placeholder="Isi Nama Lengkap"
                    v-model="form.attendeeFullName"
                    required
                  />
                </label>
              </div>
              <div class="mb-3 form-group">
                <label for="formEmail" class="form-label"
                  >Alamat Email
                  <input
                    type="email"
                    class="form-control"
                    id="formEmail"
                    placeholder="contoh@email.com"
                    v-model="form.attendeeEmail"
                    :required="true"
                  />
                </label>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
              Tutup
            </button>
            <button type="submit" class="btn btn-primary" @click="submitForm">
              Daftar Kelas
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";

export default defineComponent({
  name: "LearningClass",
  components: {},

  data() {
    return {
      learning: [] as any[],
      errors: [] as any[],
      reg: /^(([^<>()\]\\.,;:\s@"]+(\.[^<>()\]\\.,;:\s@"]+)*)|(".+"))@(([0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/ as any,
      form: {
        classId: this.$route.params.id,
        attendeeFullName: "",
        attendeeEmail: "",
      },
    };
  },

  methods: {
    setLearning(data: any) {
      this.learning = data;
    },
    hideModal() {
      (this.$refs.modal as any).click();
    },

    submitForm() {
      this.errors = [];
      if (this.form.attendeeFullName === "") {
        this.errors.push("Nama tidak boleh kosong");
      } else if (this.form.attendeeEmail === "") {
        this.errors.push("Email tidak boleh kosong");
      } else if (!this.reg.test(this.form.attendeeEmail)) {
        this.errors.push("Masukan format email dengan benar");
      } else {
        axios
          .post("http://localhost:3000/join-class", this.form)
          .then((response) => {
            alert("Selamat kamu berhasil daftar");
            this.hideModal();
          })
          .catch((error) => {
            console.log(error.response);
          });
      }
    },
  },
  mounted() {
    const urlGet = `http://localhost:3000/learning-class/?id=${this.$route.params.id}`;

    axios
      .get(urlGet)
      .then((response) => {
        this.setLearning(response.data);
      })
      .catch((error) => {
        console.log(error);
      });
  },
});
</script>

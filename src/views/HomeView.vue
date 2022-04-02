<template>
  <div class="homeView container mt-5">
    <div class="container-fluid mb-4">
      <h3 class="text-center">
        Pilih Kelasmu
        <small class="text-muted">CSS atau JavaScript</small>
      </h3>
    </div>
    <div class="row">
      <div class="col-md">
        <div class="accordion accordion-flush" id="accordionPanelsStayOpen">
          <div
            class="accordion-item"
            v-for="(availableClass, index) in availableClasses"
            :key="availableClass.id"
          >
            <h2
              class="accordion-header"
              :id="'panelsStayOpen-heading' + index"
              :availableClass="availableClass"
            >
              <button
                class="accordion-button collapsed"
                type="button"
                data-bs-toggle="collapse"
                :data-bs-target="'#panelsStayOpen-collapse' + index"
                aria-expanded="true"
                :aria-controls="'panelsStayOpen-collapse' + index"
              >
                <strong>{{ availableClass.name }}</strong>
              </button>
            </h2>
            <div
              :id="'panelsStayOpen-collapse' + index"
              class="accordion-collapse collapse"
              :aria-labelledby="'panelsStayOpen-heading' + index"
            >
              <div class="accordion-body">
                <router-link :to="'/learning-class/' + availableClass.id">
                  <div class="d-grid gap-2">
                    <button class="btn btn-success" type="button">Ikut Kelas</button>
                  </div>
                </router-link>
              </div>
            </div>
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
  name: "HomeView",
  components: {},

  data() {
    return {
      availableClasses: [] as any[],
    };
  },

  methods: {
    setAvailableClasses(data: any) {
      this.availableClasses = data;
    },
  },

  mounted() {
    axios
      .get("http://localhost:3000/available-classes")
      .then((response) => {
        this.setAvailableClasses(response.data);
      })
      .catch((error) => {
        console.log(error);
      });
  },
});
</script>

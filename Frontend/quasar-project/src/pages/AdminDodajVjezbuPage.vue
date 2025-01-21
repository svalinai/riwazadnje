<template>
  <q-page class="flex flex-center">
    <q-card style="width: 400px">
      <q-card-section>
        <div class="text-h6">Dodaj novu vježbu</div>
      </q-card-section>

      <q-card-section>
        <q-form @submit="submitForm" ref="form">
          <q-input
            filled
            v-model="exercise.naziv"
            label="Naziv"
            type="text"
            lazy-rules
            :rules="[(val) => !!val || 'Naziv je obavezan']"
          />
          <q-input
            filled
            v-model="exercise.opis"
            label="Opis"
            type="textarea"
            lazy-rules
            :rules="[(val) => !!val || 'Opis je obavezan']"
          />
          <q-input
            filled
            v-model="exercise.kategorija"
            label="Kategorija(snaga/fleksibilnost/kardio)"
            type="text"
            lazy-rules
            :rules="[(val) => !!val || 'Kategorija je obavezna']"
          />
          <q-btn label="Dodaj" type="submit" color="primary" class="q-mt-md" />
        </q-form>
      </q-card-section>

      <q-card-section v-if="message" class="text-positive">
        {{ message }}
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import axios from "axios";

export default {
  name: "AddExercise",
  data() {
    return {
      exercise: {
        naziv: "",
        opis: "",
        kategorija: "",
      },
      message: null,
    };
  },
  methods: {
    async submitForm() {
      try {
        const response = await axios.post(
          "http://localhost:4444/api/dodajVjezbu",
          this.exercise
        );
        this.message = response.data.message;
        this.$refs.form.reset();
      } catch (error) {
        console.error("Greška prilikom dodavanja vježbe:", error);
        this.message = "Došlo je do pogreške prilikom unosa.";
      }
    },
  },
};
</script>

<style scoped>
.q-page {
  max-width: 100%;
  height: 100%;
}
</style>

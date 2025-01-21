<template>
  <q-page class="flex flex-center">
    <q-card>
      <q-card-section>
        <div class="text-h6">Prijava</div>
      </q-card-section>

      <q-card-section>
        <q-form @submit.prevent="submitForm">
          <!-- Email input -->
          <q-input v-model="email" label="Email" type="email" filled required />

          <!-- Password input -->
          <q-input
            v-model="lozinka"
            label="Lozinka"
            type="password"
            filled
            required
          />

          <!-- Submit button -->
          <q-btn
            type="submit"
            label="Prijavi se"
            color="primary"
            class="full-width"
          />
        </q-form>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
// Importiranje axios-a s osnovnom URL adresom
import axios from "axios";

const axiosInstance = axios.create({
  baseURL: "http://localhost:4444", // Provjerite je li ovaj URL ispravan
  timeout: 5000, // Možete dodati timeout da izbegnete predugo čekanje
});

export default {
  data() {
    return {
      email: "",
      lozinka: "",
    };
  },
  methods: {
    async submitForm() {
      try {
        // Pokušaj prijave sa emailom i lozinkom
        const response = await axiosInstance.post("/api/login", {
          email: this.email,
          lozinka: this.lozinka,
        });

        // Ako je prijava uspješna
        alert(response.data.message);
        // Ovdje možete pohraniti podatke o korisniku (ako je potrebno)
        // Na primjer, možete spremiti podatke u Vuex store, ili koristiti router za preusmjeravanje
        this.$router.push("/"); // Preusmjeri korisnika na početnu stranicu
      } catch (error) {
        // Obrada grešaka
        console.log(error.response); // Logiranje greške
        if (error.response) {
          alert(error.response.data.message || "Došlo je do pogreške.");
        } else {
          alert("Greška prilikom povezivanja s serverom.");
        }
      }
    },
  },
};
</script>

<style scoped>
html,
body,
.q-page {
  height: 100%;
  margin: 0;
}

.q-page {
  max-width: 400px;
  margin: 0 auto;
  background-image: url("https://images.unsplash.com/photo-1550684376-efcbd6e3f031?fm=jpg&q=60&w=3000&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8N3x8YmxhY2slMjBiYWNrZ3JvdW5kfGVufDB8fDB8fHww");
  background-size: cover;
  background-position: center;
  background-attachment: fixed;
}
</style>

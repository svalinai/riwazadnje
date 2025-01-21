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

        // Ako je prijava uspješna, provjerite ulogu korisnika
        const userRole = response.data.uloga; // Pretpostavljamo da backend vraća ulogu u odgovoru

        // Ako je korisnik 'trener', preusmjeri ga na admin layout
        if (userRole === "trener") {
          this.$router.push("/admin"); // Putanja za admin layout
        } else {
          this.$router.push("/korisnik"); // Standardna početna stranica za korisnike
        }

        // Prikazivanje poruke o uspješnoj prijavi
        alert(response.data.message);
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
  max-width: 100%;
  height: 100%;
  background-image: url("https://images.unsplash.com/photo-1550684376-efcbd6e3f031?fm=jpg&q=60&w=3000&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8N3x8YmxhY2slMjBiYWNrZ3JvdW5kfGVufDB8fDB8fHww");
  background-size: cover; /* Ovo osigurava da slika prekriva cijeli prostor */
  background-position: center center;
  background-attachment: fixed; /* Ovdje slika ostaje statična kad se stranica pomiče */
  width: 100%; /* Osigurava da pozadina pokriva cijelu širinu */
  height: 100vh; /* Visina pozadine je postavljena na cijeli viewport */
}
</style>

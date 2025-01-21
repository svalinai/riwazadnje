<template>
  <q-page class="flex flex-center">
    <q-card>
      <q-card-section>
        <div class="text-h6">Registracija</div>
      </q-card-section>

      <q-card-section>
        <q-form @submit.prevent="submitForm">
          <!-- Ime -->
          <q-input v-model="ime" label="Ime" filled required />

          <!-- Prezime -->
          <q-input v-model="prezime" label="Prezime" filled required />

          <!-- Email -->
          <q-input v-model="email" label="Email" type="email" filled required />

          <!-- Lozinka -->
          <q-input
            v-model="lozinka"
            label="Lozinka"
            type="password"
            filled
            required
          />

          <!-- Submit Button -->
          <q-btn
            type="submit"
            label="Registriraj se"
            color="primary"
            class="full-width"
          />
        </q-form>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import axios from "axios"; // Import axios

export default {
  data() {
    return {
      ime: "",
      prezime: "",
      email: "",
      lozinka: "",
      uloga: "korisnik", // Uloga je automatski postavljena na "korisnik"
    };
  },
  methods: {
    async submitForm() {
      try {
        // Provjera da li su svi podaci ispravno uneseni
        if (!this.ime || !this.prezime || !this.email || !this.lozinka) {
          alert("Sva polja su obavezna!");
          return;
        }

        // Pošaljite podatke backendu
        const response = await axios.post(
          "http://localhost:4444/api/register",
          {
            ime: this.ime,
            prezime: this.prezime,
            email: this.email,
            lozinka: this.lozinka,
          }
        );

        // Ako je registracija uspješna
        alert(response.data.message); // Ovdje koristimo odgovor koji backend šalje
        this.resetForm(); // Resetiramo formu nakon uspješne registracije
      } catch (error) {
        // Obrada greške
        if (error.response) {
          alert(error.response.data.message || "Došlo je do pogreške.");
        } else {
          alert("Greška prilikom povezivanja s serverom.");
        }
      }
    },
    resetForm() {
      this.ime = "";
      this.prezime = "";
      this.email = "";
      this.lozinka = "";
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
  background-size: cover; /* Slika prekriva cijeli prostor */
  background-position: center center;
  background-attachment: fixed; /* Slika ostaje fiksirana dok se stranica pomiče */
  width: 100%; /* Širina pozadine */
  height: 100vh; /* Visina pozadine na cijeli ekran */
}
</style>

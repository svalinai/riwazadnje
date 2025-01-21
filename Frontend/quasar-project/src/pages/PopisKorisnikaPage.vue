<template>
  <q-page class="q-pa-md">
    <q-card class="shadow-2 rounded-borders">
      <q-card-section>
        <div class="text-h5 text-primary text-center">Lista korisnika</div>
      </q-card-section>

      <q-card-section>
        <!-- Tabela za prikaz korisnika -->
        <q-table
          :rows="users"
          :columns="columns"
          row-key="id"
          flat
          bordered
          class="q-mt-md"
        >
          <template v-slot:top>
            <q-btn
              label="Osvježi podatke"
              color="primary"
              icon="refresh"
              @click="fetchUsers"
              class="q-mb-md"
            />
          </template>
        </q-table>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      // Korisnici koji će se prikazivati u tablici
      users: [],
      // Definicija kolona za tablicu
      columns: [
        {
          name: "ime",
          label: "Ime",
          align: "left",
          field: "ime",
          sortable: true,
        },
        {
          name: "prezime",
          label: "Prezime",
          align: "left",
          field: "prezime",
          sortable: true,
        },
        {
          name: "email",
          label: "Email",
          align: "left",
          field: "email",
          sortable: true,
        },
      ],
    };
  },
  methods: {
    // Metoda za dohvaćanje korisnika iz API-ja
    async fetchUsers() {
      try {
        const response = await axios.get("http://localhost:4444/api/users"); // Zamijeni URL s ispravnim
        this.users = response.data;
      } catch (error) {
        console.error("Greška prilikom dohvaćanja korisnika:", error);
        this.$q.notify({
          type: "negative",
          message:
            "Nije moguće dohvatiti korisnike. Provjerite vezu s serverom.",
        });
      }
    },
  },
  // Automatski dohvat korisnika nakon učitavanja stranice
  mounted() {
    this.fetchUsers();
  },
};
</script>

<style scoped>
.q-card {
  max-width: 800px;
  margin: 0 auto;
}
</style>

<template>
  <q-page padding>
    <q-card>
      <q-card-section>
        <q-input v-model="naziv" label="Naziv plana" outlined />
        <q-input v-model="opis" label="Opis plana" type="textarea" outlined />
        <q-select
          v-model="odabraneVjezbe"
          :options="vjezbe"
          label="Odaberite vježbe"
          outlined
          multiple
        />
      </q-card-section>
      <q-card-actions align="right">
        <q-btn label="Kreiraj plan" color="primary" @click="kreirajPlan" />
      </q-card-actions>
    </q-card>
  </q-page>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      naziv: "",
      opis: "",
      odabraneVjezbe: [], // Odabrane vježbe (cijeli objekti)
      vjezbe: [], // Popis dostupnih vježbi
    };
  },
  methods: {
    // Dohvati vježbe iz API-ja
    async fetchVjezbe() {
      try {
        const res = await axios.get("http://localhost:4444/api/vjezbe");
        this.vjezbe = res.data.map((vjezba) => ({
          label: vjezba.naziv,
          value: vjezba.ID, // ID vježbe koji će se koristiti
        }));
      } catch (err) {
        console.error("Greška pri dohvaćanju vježbi:", err);
      }
    },

    // Kreiranje plana sa samo ID-evima vježbi
    async kreirajPlan() {
      if (!this.naziv || !this.opis || this.odabraneVjezbe.length === 0) {
        this.$q.notify({
          type: "negative",
          message: "Molimo unesite naziv, opis i odaberite vježbe.",
        });
        return;
      }

      // Pretvaranje odabranih vježbi u samo ID-eve
      const vjezbeIds = this.odabraneVjezbe.map((vjezba) => vjezba.value);

      const payload = {
        naziv: this.naziv,
        opis: this.opis,
        vjezbe: vjezbeIds, // Ovdje šaljemo samo ID-eve vježbi
      };

      try {
        // Slanje podataka na backend
        const response = await axios.post(
          "http://localhost:4444/api/trening-planovi",
          payload
        );

        // Obavijest o uspjehu
        this.$q.notify({
          type: "positive",
          message: "Trening plan kreiran!",
        });

        // Resetiranje forme
        this.naziv = "";
        this.opis = "";
        this.odabraneVjezbe = [];
      } catch (err) {
        console.error("Greška pri kreiranju plana:", err);

        // Obavijest o grešci
        this.$q.notify({
          type: "negative",
          message: "Došlo je do greške pri kreiranju plana.",
        });
      }
    },
  },

  // Dohvati vježbe kad je komponenta montirana
  mounted() {
    this.fetchVjezbe();
  },
};
</script>

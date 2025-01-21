<template>
  <q-page padding>
    <q-card>
      <q-card-section>
        <div class="text-h6">POPIS DOSTUPNIH PLANOVA</div>
      </q-card-section>

      <q-card-section v-if="treningPlanovi.length === 0">
        <p>Trening planovi nisu pronađeni.</p>
      </q-card-section>

      <q-card-section v-for="plan in treningPlanovi" :key="plan.plan_id">
        <q-card>
          <q-card-section>
            <div class="text-h6">{{ plan.naziv }}</div>
            <div>{{ plan.opis }}</div>
          </q-card-section>

          <q-card-section>
            <div class="text-h6">Vježbe:</div>
            <q-list>
              <q-item v-for="vjezba in plan.vjezbe" :key="vjezba.id">
                <q-item-section>{{ vjezba.naziv }}</q-item-section>
              </q-item>
            </q-list>
          </q-card-section>
        </q-card>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      treningPlanovi: [],
    };
  },
  methods: {
    async fetchTreningPlanovi() {
      try {
        const res = await axios.get(
          "http://localhost:4444/api/trening-planovi"
        );
        this.treningPlanovi = res.data;
      } catch (err) {
        console.error("Greška pri dohvaćanju planova:", err);
        this.$q.notify({
          type: "negative",
          message: "Došlo je do greške pri dohvaćanju planova!",
        });
      }
    },
  },
  mounted() {
    this.fetchTreningPlanovi();
  },
};
</script>

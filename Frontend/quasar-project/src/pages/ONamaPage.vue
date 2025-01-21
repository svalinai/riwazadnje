<template>
  <q-page class="bg-grey-1">
    <q-card class="q-pa-lg shadow-2 rounded-borders">
      <q-card-section>
        <div class="text-h5 text-center text-primary">O nama</div>
      </q-card-section>

      <q-card-section>
        <div class="q-mb-md">
          <q-img
            src="https://www.bodybuilding.com/images/2017/february/team-bbcoms-favorite-supplements-header-v2-960x540.jpg"
            alt="O nama"
            style="max-height: 300px; object-fit: cover; border-radius: 8px"
          />
        </div>

        <div class="text-body1 q-mb-md">
          Mi smo tim strastvenih fitness entuzijasta posvećenih pružanju
          kvalitetnih treninga i savjeta za zdrav životni stil. Naša misija je
          pomoći ljudima da postignu svoje ciljeve, bilo da se radi o
          poboljšanju kondicije, mršavljenju ili izgradnji mišićne mase.
        </div>

        <div class="text-body1 q-mb-md">
          Naša web platforma pruža personalizirane trening programe, edukativne
          resurse i podršku od strane certificiranih trenera. Vjerujemo da je
          zdravlje osnovni temelj sretnog života i tu smo da vas podržimo na
          vašem putu.
        </div>
      </q-card-section>

      <q-separator />

      <q-card-section>
        <div class="text-h6 text-primary q-mb-md">Naš tim</div>
        <q-list dense>
          <q-item v-for="(member, index) in teamMembers" :key="index">
            <q-item-section>
              <q-item-label>{{ member.ime }} {{ member.prezime }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      teamMembers: [],
    };
  },
  created() {
    this.fetchTeamMembers();
  },
  methods: {
    async fetchTeamMembers() {
      try {
        const response = await axios.get("http://localhost:4444/api/treneri");
        this.teamMembers = response.data;
      } catch (error) {
        console.error("Greška pri dohvaćanju trenera:", error);
      }
    },
  },
};
</script>

<style scoped>
.q-card {
  max-width: 800px;
  margin: 0 auto;
}
</style>

<template>
  <q-page padding>
    <!-- Prikaz pretrage -->
    <q-input
      v-model="searchTerm"
      debounce="300"
      label="Pretraži vježbe"
      class="q-mb-md"
      dense
      clearable
    />

    <!-- Prikaz učitavanja dok se podaci ne učitaju -->
    <q-spinner v-if="loading" size="50px" class="text-center" />

    <!-- Main Content - Karta za svaku vježbu -->
    <q-card
      dark
      bordered
      class="bg-grey-9 my-card q-mb-md"
      v-for="vjezba in filteredVjezbe"
      :key="vjezba.ID"
    >
      <q-card-section>
        <!-- Naslov vježbe -->
        <div class="text-h4 text-weight-bolder text-center text-white q-mb-md">
          {{ vjezba.naziv }}
        </div>
      </q-card-section>

      <q-card-section class="text-h5 text-justify text-white q-mb-md">
        <!-- Opis vježbe -->
        {{ vjezba.opis }}
      </q-card-section>

      <q-card-section class="text-h6 text-center text-white">
        <!-- Kategorija vježbe -->
        <strong class="text-primary">Kategorija:</strong>
        {{ vjezba.kategorija }}
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import { ref, onMounted, computed } from "vue";
import axios from "axios";

export default {
  setup() {
    // Definiramo stanje za vježbe, učitavanje i pojam pretrage
    const vjezbe = ref([]);
    const loading = ref(true);
    const searchTerm = ref(""); // Pojam za pretragu

    // Dohvat podataka o vježbama s backend servera
    onMounted(async () => {
      try {
        const response = await axios.get("http://localhost:4444/api/vjezbe");
        vjezbe.value = response.data; // Spremamo podatke u vjezbe
      } catch (error) {
        console.error("Error fetching exercises:", error);
      } finally {
        loading.value = false; // Zaustavljamo spinner
      }
    });

    // Računamo filtrirani popis vježbi na temelju unosa pretrage
    const filteredVjezbe = computed(() => {
      return vjezbe.value.filter(
        (vjezba) =>
          vjezba.naziv.toLowerCase().includes(searchTerm.value.toLowerCase()) ||
          vjezba.opis.toLowerCase().includes(searchTerm.value.toLowerCase()) ||
          vjezba.kategorija
            .toLowerCase()
            .includes(searchTerm.value.toLowerCase())
      );
    });

    return {
      vjezbe,
      loading,
      searchTerm,
      filteredVjezbe,
    };
  },
};
</script>

<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 400px
  margin: 0 auto

.text-justify
  text-align: justify

.q-mb-md
  margin-bottom: 16px

.q-mb-lg
  margin-bottom: 24px

.text-white
  color: white

.text-primary
  color: #1e88e5

.text-center
  text-align: center
</style>

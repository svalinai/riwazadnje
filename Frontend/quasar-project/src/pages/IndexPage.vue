<template>
  <q-page class="flex flex-center" style="position: relative; height: 100vh">
    <!-- Slika -->
    <img
      alt="naslovna slika"
      src="~assets/trening.jpg"
      style="width: 100%; height: 100%; object-fit: cover"
    />

    <!-- Tekst uokviren u središtu slike -->
    <q-card
      class="text-center q-pa-md"
      style="
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        max-width: 80%;
        background-color: rgba(255, 255, 255, 0.7);
        border-radius: 8px;
      "
    >
      <q-card-section>
        <!-- Naslov -->
        <h2 class="welcome-title">Dobrodošli na TreningApp</h2>
        <!-- Popratni tekst -->
        <p>Ovdje započinje vaša avantura! Pridružite nam se! .</p>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script setup>
defineOptions({
  name: "IndexPage",
});
</script>

<style scoped>
/* Koristimo Google Font: Roboto */
.welcome-title {
  font-family: "Roboto", sans-serif;
  font-weight: 500;
  color: #1976d2;
}

/* Opcionalno možete dodati dodatne stilove ovdje ako su potrebni */
</style>

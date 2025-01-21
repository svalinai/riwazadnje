<template>
  <q-page
    class="row items-center justify-center"
    style="
      background-image: url('https://images.unsplash.com/photo-1550684376-efcbd6e3f031?fm=jpg&q=60&w=3000&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8N3x8YmxhY2slMjBiYWNrZ3JvdW5kfGVufDB8fDB8fHww');
      background-size: cover;
      height: 100vh;
    "
  >
    <div
      class="overlay"
      style="
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, 0.4);
      "
    ></div>

    <!-- Centrirani tekst na vrhu -->
    <div
      class="text-center"
      style="
        position: absolute;
        top: 20px;
        width: 100%;
        color: white;
        font-size: 40px;
        font-weight: bold;
      "
    >
      Vaše mišljenje nam je bitno
    </div>

    <!-- Forma sa unosom -->
    <q-form
      @submit="submitReview"
      class="q-gutter-md"
      style="
        z-index: 1;
        position: relative;
        width: 400px;
        padding: 20px;
        border-radius: 10px;
        background-color: rgba(255, 255, 255, 0.8);
        margin-top: 100px;
      "
    >
      <q-input
        v-model="name"
        label="Ime"
        required
        autofocus
        filled
        class="q-mb-md"
      />
      <q-input
        v-model="review"
        label="Recenzija"
        required
        type="textarea"
        filled
        class="q-mb-md"
      />
      <q-btn label="Pošaljite" type="submit" color="primary" />

      <!-- Poruka nakon uspješnog slanja -->
      <div v-if="isSubmitted" class="q-mt-md text-center text-positive">
        Uspješno ste poslali recenziju!
      </div>
    </q-form>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      review: "",
      isSubmitted: false, // Flag za praćenje statusa slanja recenzije
    };
  },
  methods: {
    async submitReview() {
      try {
        const response = await this.$axios.post(
          "http://localhost:4444/api/reviews",
          {
            name: this.name,
            review: this.review,
          }
        );
        if (response.status === 200) {
          this.isSubmitted = true; // Postavi isSubmitted na true nakon uspješnog slanja
          this.$q.notify({
            type: "positive",
            message: "Recenzija je uspješno poslana!",
          });
        }
      } catch (error) {
        this.$q.notify({
          type: "negative",
          message: "Došlo je do pogreške pri slanju recenzije.",
        });
      }
    },
  },
};
</script>

<style scoped>
.q-page {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-image: url("https://images.unsplash.com/photo-1550684376-efcbd6e3f031?fm=jpg&q=60&w=3000&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8N3x8YmxhY2slMjBiYWNrZ3JvdW5kfGVufDB8fDB8fHww");
  background-size: cover;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.4);
}

.text-center {
  position: absolute;
  top: 20px; /* Pomaknut tekst na vrh */
  width: 100%;
  text-align: center;
  color: white;
  font-size: 40px;
  font-weight: bold;
}

.q-form {
  z-index: 1;
  position: relative;
  width: 400px;
  padding: 20px;
  border-radius: 10px;
  background-color: rgba(255, 255, 255, 0.8);
  margin-top: 100px; /* Razmak između teksta i formi */
}

.q-input.filled {
  background-color: white;
  border-radius: 5px;
}

.q-btn {
  width: 100%;
}

.text-positive {
  color: green; /* Dodano boje za uspješno poslanje */
}
</style>

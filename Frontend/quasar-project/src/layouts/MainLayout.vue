<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title class="text-weight-bolder text-h4">
          TreningApp
        </q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      <q-list>
        <q-item-label header> Essential Links </q-item-label>

        <!-- Render navigation links from linksList -->
        <q-item
          v-for="link in linksList"
          :key="link.title"
          clickable
          @click="goToPage(link.link)"
        >
          <q-item-section avatar>
            <q-icon :name="link.icon" />
          </q-item-section>
          <q-item-section>
            <div>{{ link.title }}</div>
            <div class="text-caption">{{ link.caption }}</div>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";

const linksList = [
  {
    title: "Naslovnica",
    caption: "početna stranica",
    icon: "home",
    link: "/", // home page path
  },

  {
    title: "O nama",
    caption: "naša priča",
    icon: "info",
    link: "/oNama", // path for PopisKnjigaPage
  },
  {
    title: "Vjezbe",
    caption: "Popis svih vjezbi",
    icon: "fitness_center",
    link: "/vjezbe", // path for TraziKnjiguPage
  },

  {
    title: "Recenzije",
    caption: "Vaše mišljenje nam je bitno!",
    icon: "rate_review",
    link: "/recenzija", // path for ONamaPage
  },
  {
    title: "Prijava",
    caption: "Ukoliko imate račun",
    icon: "login",
    link: "/prijava", // path for LokacijaPage
  },
  {
    title: "Registracija",
    caption: "Ukoliko nemate račun",
    icon: "how_to_reg",
    link: "/registracija", // path for LoginPage
  },
  {
    title: "Admin",
    caption: "administratorske postavke",
    icon: "admin_panel_settings",
    link: "/adminLogin",
  },
];

const leftDrawerOpen = ref(false);
const router = useRouter();

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value;
}

function goToPage(link) {
  router.push(link); // Use router.push to navigate to the route path
}
</script>

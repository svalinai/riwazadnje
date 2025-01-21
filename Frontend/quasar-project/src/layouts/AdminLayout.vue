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
    link: "/admin", // home page path
  },
  {
    title: "Popis korisnika",
    caption: "Korisnici nase stranice",
    icon: "list",
    link: "/admin/popisKorisnika", // home page path
  },
  {
    title: "Dodaj vjezbu",
    caption: "dodavanje novih vjezbi",
    icon: "add",
    link: "/admin/AdminDodajVjezbu", // home page path
  },
  {
    title: "Kreiraj plan",
    caption: "kreiraj plan treninga",
    icon: "description",
    link: "/admin/AdminKreirajPlan", // home page path
  },

  {
    title: "logout",
    caption: "odjava",
    icon: "logout",
    link: "/", // path for LoginPage
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

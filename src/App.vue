<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="bg-purple-8">
      <q-toolbar>
        <q-toolbar-title>
          Quote Generator
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <Home />
    </q-page-container>
  </q-layout>
</template>

<script>
import Home from "@/pages/Home.vue";

export default {
  name: "LayoutDefault",

  components: {
    Home,
  },

  data() {
    return {};
  },
};
</script>

<style></style>

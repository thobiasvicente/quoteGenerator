<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="bg-purple-8">
      <q-toolbar>
        <q-toolbar-title>
          Quote Generator
        </q-toolbar-title>
        <div>
          Made by
          <a
            class="text-white"
            flat
            href="http://github.com/thobiasvicente"
            target="_blank"
            rel="noopener noreferrer"
            >Thobias VIcente</a
          >
          <q-icon
            name="fab fa-github"
            size="md"
            class="q-px-md"
            @click="openLink"
            style="cursor:pointer"
          />
        </div>
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
  methods: {
    openLink() {
      window.open("http://github.com/thobiasvicente", "_blank");
    },
  },
};
</script>

<style></style>

<template>
  <q-layout view="lHh Lpr lFf">
    <q-layout-header>
      <q-toolbar color="tertiary" class="no-shadow">
        <img src="../assets/logo-afa.svg" alt="" class="fixed-left">
        <q-toolbar-title class="absolute" style="margin-left:10%">
          AFA PWA DEMO
        </q-toolbar-title>
      </q-toolbar>
    </q-layout-header>

    <q-page-container>
      <router-view />
    </q-page-container>

     <q-layout-footer :reveal="true">
      <div class="customFooter"></div>
    </q-layout-footer>
  </q-layout>
</template>

<script>
import { QLayoutFooter } from 'quasar';

export default {
  name: 'LayoutDefault',
  components: {
    QLayoutFooter,
  },
  data() {
    return {
    };
  },
  methods: {
  },
  computed: {
  },
};
</script>

<style>
.customFooter {
  width: 100vw;
  height: 5vh;
  background-color: black;
}
</style>

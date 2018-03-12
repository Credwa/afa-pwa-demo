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
      <transition appear enter-active-class="animated slideInUp">
        <router-view />
      </transition>
      <div class="customFooter"></div>
    </q-page-container>
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
    background-color: #212121;
    position:relative;
  }
</style>

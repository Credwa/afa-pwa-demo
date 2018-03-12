<template>
  <div id="q-app">
    <!-- <q-modal v-model="infoOpened" minimized class="column items-center pwaInfo">
      <h4>This site works offline</h4>
          That's right, a website that works offline! You can also choose "Add to Home Screen" for an app icon.
      <q-btn
        color="primary"
        @click="infoOpened = false"
        label="Close"
      />
    </q-modal> -->
    <q-modal v-model="infoOpened" :content-css="{minWidth: '80vw', minHeight: '30vh'}" minimized>
  <q-modal-layout>
    <q-toolbar slot="header" color="tertiary">
      <q-btn
        flat
        round
        dense
        v-close-overlay
        icon="clear"
      />
      <q-toolbar-title>
        This site works offline
      </q-toolbar-title>
    </q-toolbar>

    <div class="layout-padding">
      <p>That's right, a website that works offline! You can also choose "Add to Home Screen" for an app icon.</p>

      <q-btn
        color="primary"
        v-close-overlay
        label="Ok, Cool"
        flat
      />
    </div>
  </q-modal-layout>
</q-modal>
    <router-view />
  </div>
</template>

<script>
import localforage from 'localforage';
import { CloseOverlay, QModalLayout } from 'quasar';

export default {
  name: 'App',
  components: {
    QModalLayout,
  },
  directives: {
    CloseOverlay,
  },
  data() {
    return {
      showInfo: false,
      infoOpened: false,
    };
  },
  methods: {
    showPWAInfo() {
      const vm = this;
      this.$q.notify({
        message: 'Ready to work offline',
        timeout: 8000,
        type: 'tertiary',
        color: 'positive',
        icon: 'cached',
        position: 'bottom',
        actions: [
          {
            label: 'Info',
            icon: 'priority_high',
            handler: () => {
              vm.infoOpened = true;
            },
          },
        ],
      });
    },
  },
  created() {
    const vm = this;
    if (
      this.$q.platform.is.chrome ||
      this.$q.platform.is.opera ||
      this.$q.platform.is.safari ||
      this.$q.platform.is.edge ||
      this.$q.platform.is.mozilla
    ) {
      localforage
        .getItem('visited')
        .then((value) => {
          if (value) {
            console.log(value);
          } else {
            this.showPWAInfo();
            localforage.setItem('visited', 'true');
            vm.showInfo = true;
            setTimeout(() => {
              vm.showInfo = false;
            }, 2000);
          }
        })
        .catch((e) => {});
    }
  },
};
</script>

<style>
body {
  overflow-x: hidden;
}

.pwaInfo {
  background-color: red;
}
</style>

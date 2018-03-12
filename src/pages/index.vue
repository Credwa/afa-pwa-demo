<template>
  <q-page class="column items-center" style="top: 5vh">
    <!-- <p class="caption">Search Customer</p> -->
    <div class="row" style="z-index:10">
      <q-search float-label="Search Customer" style="right: 10%; width: 60vw"
      v-model="search" clearable
      >
    <q-popover style="width: 60vw" max-height="30vh">
    <!--
      The DOM element(s) that make up the popup,
      in this case a list:
    -->
    <q-list separator link>
      <!-- notice `v-close-overlay` which closes popover -->
      <q-item  v-close-overlay v-for="customer in customers" :key="customer.CustomerId"  @click.native="search = customer.CustomerName" color="blue">
        {{customer.CustomerName}}
      </q-item>
    </q-list>
  </q-popover >
      </q-search>
      <q-btn color="primary" style="max-height:5vh; margin-top:2vh" @click="getCustomerData" :loading="viewLoading">View
        <q-spinner-facebook slot="loading" />
      </q-btn>
    </div>
  <customer :customerData = "customerData"> </customer>

  </q-page>
</template>

<script>
import {
  QSearch,
  QBtn,
  QField,
  QInput,
  QPopover,
  QList,
  QItem,
  CloseOverlay,
  QSpinnerFacebook,
} from 'quasar';
import axios from 'axios';
import _ from 'lodash';

import Customer from './../components/Customer';

export default {
  components: {
    QSearch,
    QBtn,
    QField,
    QInput,
    Customer,
    QPopover,
    QList,
    QItem,
    QSpinnerFacebook,
  },
  directives: {
    CloseOverlay,
  },
  name: 'PageIndex',
  data() {
    return {
      search: '',
      customers: null,
      viewLoading: false,
      customerData: {
        CustomerName: '',
        CustomerBillPrimary: {
          Address1: '',
          Address2: '',
          City: '',
          State: '',
          Zip: '',
        },
      },
    };
  },
  methods: {
    getCustomerData() {
      this.viewLoading = true;
      setTimeout(() => {
        this.viewLoading = false;
      }, 3000);
      let foundCustomer = null;
      const tempArr = this.customers.filter(
        customer => customer.CustomerName === this.search,
      );
      foundCustomer = tempArr[0];
      const vm = this;
      axios
        .get(
          `https://sedonaapibeta.afap.com/api/customer/${
            foundCustomer.CustomerId
          }`,
        )
        .then((res) => {
          vm.customerData = res.data;
          setTimeout(() => {
            this.viewLoading = false;
          }, 500);
        })
        .catch((e) => {
          console.log(e);
          this.$q.notify({
            color: 'negative',
            textColor: 'white',
            icon: 'warning',
            message: 'Can\'t get customer data',
            position: 'bottom',
            timeout: 3000,
          });
        });
    },
    getSearchResults: _.debounce((newSearch, vm) => {
      axios
        .get(
          `https://sedonaapibeta.afap.com/api/customer?$filter=contains(CustomerName, '${newSearch}')&$select=CustomerName,CustomerId&$top=25`,
        )
        .then((res) => {
          vm.customers = res.data;
        })
        .catch((e) => {
          vm.customers = [{ CustomerName: 'Not Found' }];
        });
    }, 500),
  },
  watch: {
    search(newSearch) {
      if (newSearch.length > 3) {
        this.getSearchResults(newSearch.trim(), this);
      }
    },
  },
  computed: {},
};
</script>

<style scoped>

</style>

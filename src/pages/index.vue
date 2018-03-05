<template>
  <q-page class="column items-center" style="top: 10vh">
    <!-- <p class="caption">Search Customer</p> -->
    <div class="row" style="z-index:10">
      <q-search float-label="Search Customer" style="right: 10%; width: 60vw"
      v-model="search" clearable
      >
    <q-popover style="width: 60vw">
    <!--
      The DOM element(s) that make up the popup,
      in this case a list:
    -->
    <q-list separator link>
      <!-- notice `v-close-overlay` which closes popover -->
      <q-item v-for="customer in filteredCustomers" :key="customer.name + customer.address"  @click.native="search = customer.name">
        {{customer.name}}
      </q-item>
    </q-list>
  </q-popover >
      </q-search>
      <q-btn color="primary" @click="getCustomerData">View</q-btn>
    </div>
  <customer :customerData = "customerClicked"> </customer>
  </q-page>
</template>

<script>
import { QSearch, QBtn, QField, QInput, QPopover, QList, QItem } from 'quasar';

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
  },
  name: 'PageIndex',
  data() {
    return {
      search: '',
      showPopover: true,
      customers: [
        {
          name: 'Craig Edwards',
          address: '5512 111th st',
          apartment: '3D',
          city: 'Flushing',
          state: 'NY',
          zip: '11368',
          notes: '',
        },
        {
          name: 'App Tester',
          address: '54.164.87.50',
          apartment: 'ec2',
          city: 'aws',
          state: 'amazon',
          zip: '01010',
          notes: 'This is a test note',
        },
      ],
      customerClicked: {},
    };
  },
  methods: {
    getCustomerData() {
      let foundCustomer = null;
      const tempArr = this.customers.filter(customer => customer.name === this.search);

      foundCustomer = tempArr[0];
      this.customerClicked = foundCustomer;
    },
  },
  computed: {
    filteredCustomers() {
      let tempCustomers = [];
      if (this.search.length > 0) {
        const reg = new RegExp(`${this.search.toUpperCase()}`);
        tempCustomers = this.customers.filter(
          customer => !customer.name.toUpperCase().search(reg),
        );
      }
      return tempCustomers;
    },
  },
};
</script>

<style scoped>

</style>

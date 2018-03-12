<template>
  <div :class="!this.$q.platform.is.edge ? 'customer-info column items-center' : 'customer-info'" :style="this.$q.platform.is.edge ? 'padding-left: 10vw': ''">
    <q-field
      no-icon
    >
      <q-input float-label="Customer Name" v-model="customerData.CustomerName"
       style="width: 77vw; margin-bottom: 5%;"
       disable placeholder="Customer Name"/>
    </q-field>

    <q-field
      no-icon
    >
      <q-input float-label="Address" v-model="customerData.CustomerBillPrimary.Address1"
       style="width: 77vw; margin-bottom: 3%;" disable placeholder="Address"/>
      <q-input float-label="Address 2" v-model="customerData.CustomerBillPrimary.Address2"
       style="width: 77vw; margin-bottom: 3%;"
      disable placeholder="Apartment #"/>
      <div class="row">
        <q-input float-label="City"  v-model="customerData.CustomerBillPrimary.City"
         style="width: 30vw; margin-right: 1vw;"
        disable placeholder="City"/>
        <q-input float-label="State" v-model="customerData.CustomerBillPrimary.State"
        style="width: 25vw; margin-right: 1vw"
        disable placeholder="State"/>
        <q-input float-label="Zip Code" v-model="customerData.CustomerBillPrimary.Zip"
        style="width: 20vw" disable placeholder="Zip Code"/>
      </div>
    </q-field>

    <q-field
      no-icon
    >
      <q-input v-model="customer.notes" float-label="Notes" type="textarea"
      style="width: 77vw; margin-bottom: 10%; margin-top: 5%" clearable @click="scrollIntoView" ref="notes"
      />
    </q-field>

  <div class="row" :style="this.$q.platform.is.edge ? 'margin-left: 10vw' : ''">
    <q-btn color="info" style="right: 5%; max-height: 5vh; top:1vh" icon="edit_location" @click="getLocation">Find Me</q-btn>
    <q-input float-label="latitude" style="width:25vw;" v-model="coords.latitude"
       disable/>
    <q-input float-label="longitude" v-model="coords.longitude" style="width:25vw; left: 5%"
       disable/>
  </div>

  <q-btn color="secondary" :style="this.$q.platform.is.edge ? 'margin-top:5vh; width: 150px; margin-bottom: 15vh; margin-left: 35vw' : 'margin-top:5vh; width: 150px; margin-bottom: 5vh; '"
  @click="updateCustomerInfo" :loading="saveLoading" :disabled="customer.notes.length < 1">Save
     <q-spinner-facebook slot="loading" />
  </q-btn>

  </div>

</template>

<script>
import { QBtn, QField, QInput, Notify, QSpinnerFacebook } from 'quasar';
import axios from 'axios';

export default {
  components: {
    QBtn,
    QField,
    QInput,
    Notify,
    QSpinnerFacebook,
  },
  props: ['customerData'],
  name: 'Customer',
  data() {
    return {
      search: '',
      saveLoading: false,
      coords: {
        latitude: 0,
        longitude: 0,
      },
      customer: {
        name: '',
        address: '',
        apartment: '',
        city: '',
        state: '',
        zip: '',
        notes: '',
      },
    };
  },
  methods: {
    updateCustomerInfo() {
      this.saveLoading = true;
      setTimeout(() => {
        this.saveLoading = false;
      }, 3000);

      const customerNote = {
        CustomerId: this.customerData.CustomerId,
        UserName: this.customerData.CustomerName,
        Notes: this.customer.notes,
      };
      axios
        .post(
          'https://sedonaapibeta.afap.com/api/CustomerNote',
          customerNote,
        )
        .then((res) => {
          setTimeout(() => {
            this.saveLoading = false;
            this.$q.notify({
              color: 'secondary',
              textColor: 'white',
              icon: 'done',
              message: 'Saved Note',
              position: 'bottom',
              timeout: 3000,
            });
          }, 500);
          console.log(res);
        })
        .catch((e) => {
          console.log(e);
          this.$q.notify({
            color: 'negative',
            textColor: 'white',
            icon: 'warning',
            message: 'Could not save note',
            position: 'bottom',
            timeout: 3000,
          });
        });
    },
    scrollIntoView() {
      this.$refs.notes.$el.scrollIntoView({ behavior: 'smooth' });
    },
    getLocation() {
      const self = this;
      if (!navigator.geolocation) {
        this.$q.notify({
          color: 'negative',
          textColor: 'white',
          icon: 'warning',
          message: 'Geolocation not supported by your browser',
          position: 'bottom',
          timeout: 3000,
        });
      }
      navigator.geolocation.getCurrentPosition(
        (position) => {
          self.coords.latitude = position.coords.latitude;
          self.coords.longitude = position.coords.longitude;
        },
        (err) => {
          console.log(err);
          this.$q.notify({
            color: 'negative',
            textColor: 'white',
            icon: 'warning',
            message: 'Unable to get location',
            position: 'bottom',
            timeout: 3000,
          });
        },
      );
      return 0;
    },
  },
};
</script>

<style scoped>
.customer-info {
  width: 100vw;
  background-color: #eeeeee;
  position: relative;
  min-height: 75vh;
  margin-top: 10vh;
  padding: 5%;
}

.coords {
  display: flex;
  flex-direction: row;
}
</style>

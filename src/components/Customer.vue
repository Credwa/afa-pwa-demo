<template>
  <div class="customer-info column items-center">
    <q-field
      no-icon
    >
      <q-input float-label="Customer Name" v-model="customerData.name"
       style="width: 77vw; margin-bottom: 5%;"
       disable placeholder="Customer Name"/>
    </q-field>

    <q-field
      no-icon
    >
      <q-input float-label="Address" v-model="customerData.address"
       style="width: 77vw; margin-bottom: 3%;" disable placeholder="Address"/>
      <q-input float-label="Apartment/Suite" v-model="customerData.apartment"
       style="width: 77vw; margin-bottom: 3%;"
      disable placeholder="Apartment #"/>
      <div class="row">
        <q-input float-label="City"  v-model="customerData.city"
         style="width: 30vw; margin-right: 1vw;"
        disable placeholder="City"/>
        <q-input float-label="State" v-model="customerData.state"
        style="width: 25vw; margin-right: 1vw"
        disable placeholder="State"/>
        <q-input float-label="Zip Code" v-model="customerData.zip"
        style="width: 20vw" disable placeholder="Zip Code"/>
      </div>
    </q-field>

    <q-field
      no-icon
    >
      <q-input v-model="customerData.notes" float-label="Notes" type="textarea"
      style="width: 77vw; margin-bottom: 10%; margin-top: 5%" clearable/>
    </q-field>

  <div class="row">
    <q-btn color="info" style="right: 5%" icon="edit_location" @click="getLocation">Find Me</q-btn>
    <q-input float-label="latitude" style="width:25vw;" v-model="latitude"
       disable/>
    <q-input float-label="longitude" v-model="longitude" style="width:25vw; left: 5%"
       disable/>
  </div>

  <q-btn color="secondary" style="margin-top:5%; width: 150px">Save</q-btn>

  </div>

</template>

<script>
import { QBtn, QField, QInput, Notify } from 'quasar';

export default {
  components: {
    QBtn,
    QField,
    QInput,
    Notify,

  },
  props: ['customerData'],
  name: 'Customer',
  data() {
    return {
      search: '',
      latitude: 0,
      longitude: 0,
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
          self.latitude = position.coords.latitude;
          self.longitude = position.coords.longitude;
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
  bottom: 0;
  min-height: 70vh;
  margin-top: 5vh;
  padding: 5%;
}

.coords {
  display: flex;
  flex-direction: row;
}
</style>

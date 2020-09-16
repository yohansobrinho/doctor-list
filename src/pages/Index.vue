<template>
  <q-page class="flex flex-center">
    <!-- <img
      alt="Quasar logo"
      src="~assets/quasar-logo-full.svg"
    > -->
    <DoctorTable></DoctorTable>
  </q-page>
</template>

<script>

import DoctorTable from 'components/DoctorTable.vue'

export default {
  components: {
    DoctorTable
  },
  name: 'PageIndex'
}
</script>

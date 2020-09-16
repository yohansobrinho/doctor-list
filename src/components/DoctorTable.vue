<template>
  <div class="q-pa-md">
    <q-table
      title="Doctors"
      :data="dataFilter"
      :columns="columns"
      row-key="name"
      selection="multiple"
      :selected.sync="selected"
      :filter="filter"
      :pagination.sync="pagination"
      grid
      hide-header
    >
      <template v-slot:top-right>
          <div class="q-mr-md">
            <q-select
              filled
              emit-value
              map-options
              transition-show="flip-up"
              transition-hide="flip-down"
              v-model="model"
              label="Simple select"
              :options="options"
              @input="searchQSelect(model)"
              style="width: 230px"
              behavior="menu"
            >
              <template v-slot:append>
                <q-icon
                  v-if="model !== null"
                  class="cursor-pointer"
                  name="clear"
                  @click.stop="model = null"
                  @click="loadDataTable()"
                />
              </template>
            </q-select>
          </div>
        <q-input borderless dense debounce="300" v-model="filter" placeholder="Search">
          <template v-slot:append>
            <q-icon name="search" />
          </template>
        </q-input>
      </template>

      <template v-slot:item="props">
        <div
          class="q-pa-xs col-xs-12 col-sm-6 col-md-4 col-lg-3 grid-style-transition"
          :style="props.row.available ? 'transform: scale(0.95);' : ''"
        >
          <q-card :class="props.row.available ? 'bg-grey-2' : ''">
            <q-card-section>
                <q-checkbox dense v-model="props.row.available" :label="props.row.name" />
                <q-badge class="q-ml-sm"> {{ props.row.available ? 'available' : 'unavailable' }} </q-badge>
            </q-card-section>
            <q-separator />
            <q-list dense>
              <q-item v-for="col in props.cols.filter(col => col.name !== 'desc')" :key="col.name">
                <q-item-section>
                  <q-item-label>{{ col.label }}</q-item-label>
                </q-item-section>
                <q-item-section side>
                  <q-item-label caption>{{ col.value }}</q-item-label>
                </q-item-section>
              </q-item>
            </q-list>
          </q-card>
        </div>
      </template>
    </q-table>
  </div>
</template>
<script>

const stringOptions = [
  {
    label: 'Available',
    value: true
  },
  {
    label: 'Unavailable',
    value: false
  }
]

export default {
  data () {
    return {
      model: null,
      filter: '',
      dataFilter: [],
      selected: [],
      options: stringOptions,
      pagination: {
        rowsPerPage: 10
      },
      columns: [
        {
          name: 'desc',
          required: true,
          align: 'left',
          field: row => row.name,
          sortable: true
        },
        { name: 'upin', label: 'Upin', field: 'upin' },
        { name: 'zipcode', label: 'Zip code', field: 'zipcode' },
        { name: 'city', label: 'City', field: 'city' }
      ],
      data: [
        {
          name: 'John Doe',
          upin: 202029,
          zipcode: 92037,
          city: 'La Jolla',
          available: false
        },
        {
          name: 'Nick Ramsen',
          upin: 402910,
          zipcode: 92037,
          city: 'La Jolla',
          available: true
        },
        {
          name: 'Liz Redfield',
          upin: 910291,
          zipcode: 92015,
          city: 'San Diego',
          available: true
        },
        {
          name: 'Javier Garcia',
          upin: 202914,
          zipcode: 92015,
          city: 'San Diego',
          available: false
        },
        {
          name: 'Harry Bone',
          upin: 394840,
          zipcode: 92015,
          city: 'San Diego',
          available: true
        },
        {
          name: 'Kevin Lamkin',
          upin: 982170,
          zipcode: 92015,
          city: 'San Diego',
          available: true
        },
        {
          name: 'Andrew Stuart',
          upin: 393920,
          zipcode: 92037,
          city: 'La Jolla',
          available: true
        },
        {
          name: 'Maggie Willians',
          upin: 655942,
          zipcode: 92015,
          city: 'San Diego',
          available: true
        }
      ]
    }
  },
  mounted () {
    this.loadDataTable()
  },
  computed: {
  },
  methods: {
    loadDataTable () {
      this.dataFilter = this.data
    },
    searchQSelect (model) {
      this.dataFilter = this.data.filter(e => e.available === model)
    }
  }
}
</script>
<style lang="sass">
  .grid-style-transition
    transition: transform .28s, background-color .28s
</style>

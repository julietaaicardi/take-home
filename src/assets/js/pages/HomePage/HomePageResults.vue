<template>
  <div class="home-page-results w-full flex justify-center px-4">
    <div class="container flex flex-col self-end">
      <HomePageFilters
        @change="onChange"
      />
      <div class="grid md:grid-cols-3 gap-4 md:pt-10">
        <ResultBox
          v-for="(office, index) in officesList"
          :key="index"
          :city="office.city"
          :country="office.country"
          :weather="office.weather"
          :flight-price="office.flightPrice"
        />
      </div>
    </div>
  </div>
</template>

<script>
import HomePageFilters from './HomePageFilters.vue'
import ResultBox from '../../components/ResultBox.vue'

export default {
  name: 'HomePageResults',
  components: {
    HomePageFilters,
    ResultBox,
  },
  props: {
    offices: {
      type: Array,
      default: () => ([])
    },
  },
  data: () => ({
    sortedOffices: null,
    sort: '',
  }),
  computed: {
    officesList () {
      return this.sortedOffices || this.offices
    },
  },
  methods: {
    onChange (sortValue) {
      if (this.sort !== sortValue) {
        this.sortedOffices = this.sortOffices(sortValue, this.offices)
      }
    },
    sortOffices (sortValue, offices) {
      switch (sortValue) {
        case 'weather':
          return offices.sort((a, b) => {
            return b.weather.temp - a.weather.temp
          })
          break

        case 'flight':
          return offices.sort((a, b) => {
            return a.flightPrice - b.flightPrice
          })
          break

        case 'both':
          return offices.sort((a, b) => {
            return b.weather.temp - a.weather.temp || a.flightPrice - b.flightPrice
          })
          break
      }
    }
  }
}
</script>

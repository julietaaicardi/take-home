<template>
  <div class="home-page-search block bg-teal-400 block justify-center px-4 pt-8 h-24 md:px-0 md:pt-0 md:h-auto md:flex">
    <div class="container bg-white rounded-2xl border-solid border border-gray-200 pt-10 pb-6 px-4 flex flex-col items-center
      md:flex-row md:transform md:translate-y-1/2 md:translate-x-0 md:px-8 md:py-10">
    <div class="w-full md:w-auto mb-4 md:mb-0 md:mr-32">
      <h1
        class="font-rubik text-3xl text-teal-500"
        v-text="'Welcome back!'"
      />
      <h2
        class="font-karla text-xl text-gray-500 font-bold"
        v-text="'Find the best office location'"
      />
    </div>
      <div class="w-full md:w-auto md:flex-grow">
        <form @submit.prevent="onSearch" class="relative">
          <label
            for="from"
            v-text="'I\'M TRAVELLING FROM'"
            class="font-karla text-sm text-gray-600 font-bold block pb-1"
          />
          <input
            v-model="location"
            type="text"
            name="from"
            id="from"
            placeholder="City"
            list="cities"
            class="rounded-md border-solid border text-gray-900 border-gray-200 p-2 w-full md:w-8/12 md:mr-4"
            @keyup="onKeyUp"
          >
          <span
            v-if="locationError"
            v-text="locationError"
            class="absolute block font-karla text-md text-red-600 md:mt-2"
          />
          <datalist id="cities">
            <option
              v-for="(city, index) in cities"
              :key="index"
              v-text="city.text"
            />
          </datalist>
          <Button
            text="Search"
            class="rounded-full bg-teal-400 hover:bg-teal-500 text-white py-3 px-6 w-full mt-6 md:mt-0 md:py-2 md:w-auto"
          />
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import Button from '../../components/Button.vue'

export default {
  name: 'HomePageSearch',
  components: {
    Button,
  },
  data: () => ({
    location: null,
    cities: null,
    selectedLocation: null,
    locationError: null,
    offices: [
      {
        city: 'Amsterdam',
        country: 'Netherlands',
        code: 'AMS',
        weather: null,
        flightPrice: null,
      },
      {
        city: 'Budapest',
        country: 'Hungary',
        code: 'BUD',
        weather: null,
        flightPrice: null,
      },
      {
        city: 'Madrid',
        country: 'Spain',
        code: 'MAD',
        weather: null,
        flightPrice: null,
      },
    ],
    WEATHER_API_KEY: 'e1a9edf39d58029ea4e6df01724ba9df'
  }),
  computed: {
    currentDate () {
      return new Date().toLocaleDateString()
    },
  },
  watch: {
    // check that the input value matches with a city option
    // this is to prevent to modify the input value after selecting an city option
    location: function (newVal) {
      if (this.cities) {
        this.selectedLocation = this.cities.find(city => {
          return city.text.toLowerCase() === newVal.trim().toLowerCase()
        }) || null
      }
    }
  },
  methods: {
    onKeyUp () {
      if (this.location.length >= 3 && this.location.length <= 5) this.displayCityOptions()
    },
    displayCityOptions () {
      const parameters = `term=${this.location}&locale=en-US&location_types=city&limit=5&active_only=true&sort=name`

      fetch(`https://api.skypicker.com/locations?${parameters}`, { "method": "GET"})
      .then(response => response.json())
      .then(response => {
        if (response.locations.length) {
          this.cities = response.locations.map(location => {
            return {
              city: location.name,
              country: location.country.name,
              text: `${location.name}, ${location.country.name}`,
              code: location.code
            }
          })
        }
      })
      .catch(err => {
        console.error(err)
      })
    },
    async onSearch () {
      this.handleLoading(true)
      this.handleValidations()

      if (this.locationError) {
        this.handleLoading(false)
        return
      }
      
      await Promise.all([
        this.getFlightPrices(),
        this.getCurrentWeather(),
      ])
      .then(() => {
        this.$emit('search', { result: this.offices, loading: false })
      })
      .catch(err => {
        console.error(err)
        this.handleError('There was a problem, please try later')
        this.handleLoading(false)
      })
    },
    handleLoading (bool) {
      this.$emit('loading', bool)
    },
    handleError (str) {
      this.$emit('error', str)
    },
    handleValidations () {
      this.locationError = ''
      
      if (!this.location) {
        this.locationError = "Please select a city"
      }
            
      if (this.location && !this.selectedLocation) {               
        this.locationError = "Please select a valid city"
      }

      if (this.selectedLocation && !this.selectedLocation.code) {               
        this.locationError = "Please select a city with airport"
      }

      if (this.selectedLocation && this.selectedLocation.code && this.offices.some(office => office.code === this.selectedLocation.code)) {
        this.locationError = "That is one of ours HQ cities"
      }
    },
    getFlightPrices () {
      const requests = this.offices.map(office => {
        const parameters = `fly_from=${this.selectedLocation.code}&fly_to=${office.code}&date_from=${this.currentDate}&partner=picky&flight_type=oneway`
        return fetch(`https://api.skypicker.com/flights?${parameters}`, { "method": "GET"})      
      })

      return Promise.all(requests)
      .then(responses => Promise.all(responses.map(r => r.json())))
      .then(responses => {
        Object.keys(responses).forEach(key => {
          const firstResult = responses[key].data[0]
          this.offices[key].flightPrice = firstResult.price || ''
        })
      })
      .catch(err => {
        console.error(err)
      })
    },
    getCurrentWeather () {
      const requests = this.offices.map(office => {
        const cityQuery = office.city.toLowerCase().split(' ').join('+') || ''
        const parameters = `q=${cityQuery}&appid=${this.WEATHER_API_KEY}&units=metric`
        return fetch(`https://api.openweathermap.org/data/2.5/weather?${parameters}`, { "method": "GET"})      
      })

      return Promise.all(requests)
      .then(responses => Promise.all(responses.map(r => r.json())))
      .then(responses => {
        Object.keys(responses).forEach(key => {
          this.offices[key].weather = {
            temp: Math.trunc(responses[key].main.temp) || '',
            temp_min: Math.trunc(responses[key].main.temp_min) || '',
            temp_max: Math.trunc(responses[key].main.temp_max) || '',
            icon: responses[key].weather[0].icon || '',
          }
        })
      })
      .catch(err => {
        console.error(err)
      })
    },
  }
}
</script>

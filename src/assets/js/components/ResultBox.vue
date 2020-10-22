<template>
  <div class="border-solid border border-gray-200 rounded-md overflow-hidden relative">
    <div class="bg-teal-400 py-2 px-4 flex items-center justify-center rounded-full absolute top-3 right-3">
      <svg class="w-4 h-4 mr-2" viewBox="0 0 20 20" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
        <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
          <g id="icon-shape" class="fill-current text-white">
            <path d="M12,11.6 L12,17.2 L15,19 L15,20 L10,20 L5,20 L5,19 L8,17.2 L8,11.6 L-8.8817842e-16,14 L-8.8817842e-16,12 L8,7.2 L8,1.99508929 C8,0.893231902 8.88772964,0 10,0 C11.1045695,0 12,0.892622799 12,1.99508929 L12,7.2 L20,12 L20,14 L12,11.6 Z" id="Combined-Shape" transform="translate(10.000000, 10.000000) rotate(-270.000000) translate(-10.000000, -10.000000) "></path>
          </g>
        </g>
      </svg>
      <span
        class="font-karla text-md text-white font-bold"
        v-text="`€ ${flightPrice}`"
      />
    </div>
    <div class="overflow-hidden h-40 md:h-64">
      <img class="fit-picture"
        :src="imageSrc"
        :alt="city"
      >
    </div>
    <div class="p-4 flex justify-between">
      <div class="flex flex-col justify-center">
        <h3
          v-text="city"
          class="font-rubik text-2xl text-teal-500 p-0"
        />
        <h2
          v-text="country"    
          class="font-rubik text-md text-teal-600 font-bold -mt-2"
        />
      </div>
      <div class="text-right -mt-2">
        <img
          :src="`http://openweathermap.org/img/wn/${weather.icon}@2x.png`"
          width="50px"
          height="50px"
          alt="weather icon"
          class="inline"
        >
        <span
          v-text="`${weather.temp}°`"
          class="font-rubik text-2xl text-teal-600 font-light align-bottom inline-block mb-1"
        />
        <span
          v-text="`min ${weather.temp_min}° - max ${weather.temp_max}°`"
          class="font-rubik text-sm text-gray-500 block -mt-2"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ResultBox',
  props: {
    city: {
      type: String,
      default: ''
    },
    country: {
      type: String,
      default: ''
    },
    weather: {
      type: Object,
      default: () => ({})
    },
    flightPrice: {
      type: Number,
      default: 0
    },
  },
  computed: {
    imageSrc () {
      const city = this.city.toLowerCase().split(' ').join('+')
      return require(`../../images/office-${city}.jpg`)
    },
  },
}
</script>

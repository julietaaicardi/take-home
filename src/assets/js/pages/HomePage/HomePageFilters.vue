<template>
  <div class="home-page-filters w-full flex justify-end md:border-solid md:border-b md:border-gray-200 pb-4">
    <label
      v-text="'Sort on'"
      for="sort"
      class="font-karla text-sm text-gray-600 font-bold p-2"
    />
    <select
      v-model="sort"
      id="sort"
      class="font-karla text-sm text-gray-600 rounded-md p-2 border-solid border border-gray-200"
      @change="onChange"
    >
      <option
        v-for="(option, index) in options"
        :key="index"
        v-text='option.text'
        :value='option.value'
        :disabled='option.disabled'
      />
    </select>
  </div>
</template>

<script>
export default {
  name: 'HomePageFilters',
  data: () => ({
    sort:'select',
  }),
  computed: {
    options () {
      return [
        {
          text: 'Select option',
          disabled: true,
          value: 'select',
        },
        {
          text: 'Best weather',
          value: 'weather',
        },
        {
          text: 'Cheapest flight',
          value: 'flight',
        },
        {
          text: 'Both options',
          value: 'both',
        },
      ]
    },
  },
  methods: {
    onChange () {
      this.$emit('change', this.sort)
    },
  }
}
</script>

<template>
  <div class="home-page">
    <HomePageSearch
      @loading="onLoading"
      @search="onSearch"
    />
    <div class="mt-64 md:mt-32">
      <Loading v-if="loading"/>
      <HomePageEmpty v-else-if="emptyStateVisible"/>
      <HomePageResults
        v-else
        :offices="offices"
      />
    </div>
  </div>
</template>

<script>
import HomePageSearch from './HomePageSearch.vue'
import HomePageEmpty from './HomePageEmpty.vue'
import HomePageResults from './HomePageResults.vue'
import Loading from '../../components/Loading.vue'

export default {
  name: 'HomePage',
  components: {
    HomePageSearch,
    HomePageEmpty,
    HomePageResults,
    Loading,
  },
  data: () => ({
    offices: [],
    loading: false,
  }),
  computed: {
    emptyStateVisible () {
      return !this.loading && this.offices.length === 0
    }
  },
  methods: {
    onLoading (bool) {
      this.loading = bool
    },
    onSearch ({ result, loading }) {
      this.loading = loading
      this.offices = result
    }
  }
}
</script>

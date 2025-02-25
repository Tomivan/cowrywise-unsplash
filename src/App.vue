<template>
  <SearchPhoto @search-results="handleSearchResults" @reset="handleResetSearch" />
  <ImageCard ref="imageCard" />
</template>

<script>

import ImageCard from './components/ImageCard.vue'
import SearchPhoto from './components/SearchPhoto.vue';

export default {
  name: 'App',
  components: {
    ImageCard,
    SearchPhoto
  },
  methods: {
    handleSearchResults(results) {
      this.$refs.imageCard.updatePhotos(results);
    },
    handleResetSearch() {
      this.$refs.imageCard.getUnsplashPhotos();
    }
  }
}
</script>

<style>

</style>

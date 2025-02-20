<template>
  <SearchPhoto />
  <ImageCard />
</template>

<script>
import ImageCard from './components/ImageCard.vue'
import SearchPhoto from './components/SearchPhoto.vue';

export default {
  name: 'App',
  components: {
    ImageCard,
    SearchPhoto
  }
}
</script>

<style>

</style>

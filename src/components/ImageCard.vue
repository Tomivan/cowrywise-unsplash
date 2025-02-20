<template>
  <section class="image-card">

      <template v-if="loading">
        <VueContentLoading v-for="i in 6" :key="i">
          <rect x="0" y="0" rx="5" ry="5" width="200" height="250" />
          <rect x="0" y="260" rx="5" ry="5" width="150" height="20" />
          <rect x="0" y="290" rx="5" ry="5" width="100" height="20" />
        </VueContentLoading>
      </template>

      <template v-else>
        <div class="card" v-for="(item, index) in photos" :key="{index}">
            <img :src="item.urls.small" alt="" class="photo">
            <div class="details">
                <p>{{item.user.name}}</p>
                <p>{{item.user.location}}</p>
            </div>
        </div>
      </template>
    </section>
</template>

<script>
import VueContentLoading from 'vue-content-loading';

export default {
  name: 'ImageCard',
  components: {
    VueContentLoading,
  },
  props: {
    searchResults: Array, 
    loading: Boolean,
  },
   data() {
    return {
      photos: []
    }
    },
    watch: {
      searchResults: {
        immediate: true,
        handler(newResults) {
          if (newResults.length > 0) {
            this.photos = newResults;
          }
        },
      },
    },
    created() {
      this.getUnsplashPhotos()
  },
   methods: {
    getUnsplashPhotos() {
     const headers = { "Authorization": "Client-ID J05MCgl8YDTlnEQRvSyg4mKnaEEU6iC6GSgc6etlexU"};
  fetch("https://api.unsplash.com/photos",  { headers })
    .then(response => response.json())
    .then(data => {
        this.photos = data;
    })
    .catch(error => console.log('error: ', error))
  },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.image-card{
  display: flex;
  flex-wrap: wrap;
  margin: -1% 0 0 23%;
  width: 60%;
}
.card{
  margin: 0 2% 2% 0;
}
.photo{
  width: 16vw;
  height: 40vh;
  border-radius: 5px;
}

@media screen and (max-width: 768px){
  .photo{
    width: 60vw;
  }
  .image-card{
    margin-left: 20%;
  }
}
</style>

<template>
  <section>

      <div v-if="loading">
        <vue-skeleton-loader :width="200" :height="100" />
      </div>

      <div v-else>
        <div v-if="photos.length > 0" class="image-card">
          <div class="card" v-for="(item, index) in photos" :key="index">
            <div class="overlay"></div>
            <img :src="item.urls?.small" alt="Image" class="photo">
            <div class="details">
              <p>{{ item.user?.name || 'Unknown' }}</p>
              <p>{{ item.user?.location || 'Unknown' }}</p>
            </div>
          </div>
        </div>

      <!-- Handle Empty State -->
      <p v-else>No images found.</p>
    </div>
    </section>
</template>

<script>
import VueSkeletonLoader from "vue-skeleton-loader";

export default {
  name: 'ImageCard',
  components: {
    VueSkeletonLoader,
  },
  props: {
    searchResults: {
      type: Array,
      default: () => []
    }, 
  },
   data() {
    return {
      photos: [],
      loading: true
    }
    },
    watch: {
      searchResults: {
        immediate: true,
        handler(newResults) {
            this.photos = newResults || [];
        },
      },
    },
    created() {
      this.getUnsplashPhotos()
  },
   methods: {
    getUnsplashPhotos(query = "") {
    this.loading = true;
    const url = query
        ? `https://api.unsplash.com/search/photos?query=${query}`
        : "https://api.unsplash.com/photos";
    fetch(url, {
      headers: { "Authorization": "Client-ID J05MCgl8YDTlnEQRvSyg4mKnaEEU6iC6GSgc6etlexU" }
    })
    .then(response => response.json())
    .then(data => {
      this.photos = query ? data.results : data; 
      this.loading = false;
    })
    .catch(error => {
      console.error("Error fetching photos:", error);
      this.loading = false;
    });
  },
  updatePhotos(newPhotos) {
    this.photos = newPhotos
  }
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
.overlay{
  position: absolute;
  width: 16vw;
  height: 40vh;
  background: rgba(0, 0, 0, 0.5)
}
.card{
  margin: 0 2% 5% 0;
}
.photo{
  width: 16vw;
  height: 40vh;
  border-radius: 5px;
}
.details{
  position: relative;
  margin-top: -40%;
  color: #ffffff;
  z-index: 100;
  padding-left: 1em;
  font-size: 12px;
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

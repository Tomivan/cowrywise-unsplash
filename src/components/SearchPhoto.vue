<template>
    <div class="search">
       <div v-if="loading" class="input center"> Searching for "{{ search }}"</div>
       <div v-else-if="photos.length > 0" class="input center">
         Showing results for "{{ search }}" 
         <span class="pi pi-times" @click="resetSearch"></span>
       </div>
       <div v-else class="input">
          <span class="pi pi-search"></span>
          <input type="text" placeholder="Search for photo" v-model="search" @input="debouncedSearch">
        </div>
    </div>
  </template>
  
  <script>
  import "primeicons/primeicons.css";
  
  export default {
    name: "SearchPhoto",
    data() {
    return {
      photos: [],
      search: "",
      loading: false,
      debounceTimeout: null,
    };
    },
    methods: {
      searchPhotos() {
        if (!this.search.trim()) return; // Avoid empty searches

        this.loading = true;
  
        const headers = {
          Authorization: "Client-ID J05MCgl8YDTlnEQRvSyg4mKnaEEU6iC6GSgc6etlexU",
        };
  
        fetch(`https://api.unsplash.com/search/photos?query=${this.search}`, {
          headers,
        })
          .then((response) => response.json())
          .then((data) => {
            this.photos = data.results; // Store the fetched images
            this.loading = false;
            this.$emit("search-results", this.photos);
          })
          .catch((error) => {
            console.error("Error fetching photos:", error)
            this.loading = false;
        });
      },
      debouncedSearch() {
        clearTimeout(this.debounceTimeout);
        this.debounceTimeout = setTimeout(() => {
          this.searchPhotos();
        }, 1000); 
      },
      resetSearch() {
        this.search = "";
        this.photos = [];
        this.$emit("reset"); 
      }
    },
  };
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  .input{
    background: #c8c8c8;
    padding: 2em;
    height: 10vh;
  }
  input{
    height: 7vh;
    width: 65vw;
    border: 1px solid #0000001e;
    border-radius: 5px;
    margin-left: 15%;
    padding-left: 4em;
  }
  input::placeholder{
    padding-left: 2em;
  }
  .pi-search{
    position: relative;
    left: 18%;
  }
  .pi-times {
  color: red;
  cursor: pointer;
  margin-left: 10px;
  font-size: 1.2em;
}
.center {
    font-size: 24px;
    display: flex;
    justify-content: space-around;
}
  </style>
<template>
  <div v-if="isOpen" class="modal-overlay" @click.self="closeModal">
    <div class="modal-content">
      <button class="nav-button prev-button" @click="prevImage" :disabled="!hasPrev">
        &lt;
      </button>
      <div class="image-container">
        <img :src="currentImage.urls?.small" alt="Image" class="modal-image" />
        <div class="image-details">
          <p class="username">{{ currentImage.user?.name || "Unknown" }}</p>
          <p class="location">{{ currentImage.user?.location || "Unknown" }}</p>
        </div>
      </div>
      <button class="nav-button next-button" @click="nextImage" :disabled="!hasNext">
        &gt;
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isOpen: {
      type: Boolean,
      required: true,
    },
    images: {
      type: Array,
      required: true,
    },
    selectedIndex: {
      type: Number,
      required: true,
    },
  },
  computed: {
    currentImage() {
      return this.images[this.selectedIndex];
    },
    hasPrev() {
      return this.selectedIndex > 0;
    },
    hasNext() {
      return this.images && this.selectedIndex < this.images.length - 1;
    },
  },
  methods: {
    closeModal() {
      this.$emit("close-modal");
    },
    prevImage() {
      this.$emit("change-image", this.selectedIndex - 1);
    },
    nextImage() {
      this.$emit("change-image", this.selectedIndex + 1);
    },
  },
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  max-width: 90%;
  max-height: 90%;
}

.image-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.modal-image {
  max-width: 100%;
  max-height: 80vh;
  border-radius: 8px;
}

.image-details {
  margin-top: -10%;
  background: white;
  width: 100%;
  border-bottom-right-radius: 8px;
  border-bottom-left-radius: 8px;
}

.username {
  font-size: 18px;
  font-weight: bold;
  margin-left: 5%;
}

.location {
  font-size: 14px;
  color: #ccc;
  margin-left: 5%;
}

.nav-button {
  background: #ffffff;
  border: none;
  font-size: 16px;
  padding: 10px;
  cursor: pointer;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 20px;
}

.nav-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.prev-button {
  margin-right: 20px;
}

.next-button {
  margin-left: 20px;
}

</style>
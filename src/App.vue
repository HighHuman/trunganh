<script setup>
import { ref, onMounted } from "vue";

const slides = ref([]);
const currentIndex = ref(0);
const startX = ref(0);
const isDragging = ref(false);
const isTransitioning = ref(false);

// Chuyển slide
const nextSlide = () => {
  if (isTransitioning.value) return;
  if (currentIndex.value < slides.value.length - 1) {
    isTransitioning.value = true;
    currentIndex.value++;
  }
};

const prevSlide = () => {
  if (isTransitioning.value) return;
  if (currentIndex.value > 0) {
    isTransitioning.value = true;
    currentIndex.value--;
  }
};

// Bắt đầu kéo
const startDrag = (event) => {
  event.preventDefault();
  isDragging.value = true;
  startX.value = event.touches ? event.touches[0].clientX : event.clientX;
};

// Kết thúc kéo
const endDrag = (event) => {
  event.preventDefault();
  if (!isDragging.value || isTransitioning.value) return;
  isDragging.value = false;

  const endX = event.changedTouches
    ? event.changedTouches[0].clientX
    : event.clientX;
  const deltaX = endX - startX.value;

  if (deltaX > 20) prevSlide();
  if (deltaX < -20) nextSlide();
};

// Sự kiện kết thúc transition
const handleTransitionEnd = () => {
  isTransitioning.value = false;
};

onMounted(() => {
  for (let i = 1; i < 413; i++) {
    slides.value.push(
      `../public/1741097062991-648bf582-2380-43b3-b11e-e5f74c650ba1_${i}.jpg`
    );
  }
});
</script>

<template>
  <div
    class="slider"
    @mousedown="startDrag"
    @mouseup="endDrag"
    @mouseleave="endDrag"
    @touchstart="startDrag"
    @touchend="endDrag"
  >
    <div
      class="slides"
      :style="{ transform: `translateX(-${currentIndex * 100}%)` }"
      @transitionend="handleTransitionEnd"
    >
      <div v-for="(slide, index) in slides" :key="index" class="slide">
        <img :src="slide" />
      </div>
    </div>
  </div>
</template>

<style>
.slider {
  width: 100%;
  max-width: 600px;
  overflow: hidden;
  position: absolute;
  border-radius: 10px;
  top: 50%;
  transform: translateY(-50%);
}

.slides {
  display: flex;
  transition: transform 0.3s ease-in-out;
}

.slide {
  min-width: 100%;
  box-sizing: border-box;
}

img {
  width: 100%;
  display: block;
}

/* Nút điều hướng */
button {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.5);
  color: white;
  border: none;
  padding: 10px;
  cursor: pointer;
  font-size: 24px;
}
body {
  height: 100vh;
  width: 100vw;
}
/* #app {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 95%;
} */
@media screen and (min-width: 768px) {
  .slider {
    max-width: unset;
    height: 100%;
  }
  .slides {
    height: 100%;
  }
  img {
    height: 100%;
    width: auto;
  }
  .slide {
    min-width: 50%;
    box-sizing: border-box;
  }
}
</style>

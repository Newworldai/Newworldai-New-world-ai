<template>
  <div class="home">
    <cyber-cursor />
    
    <!-- 保留加载动画 -->
    <div class="loading-section">
      <loading-spinner />
    </div>

    <!-- 第一屏 -->
    <ImageCarousel />
  
  </div>
</template>

<script>
import ImageCarousel from '@/components/ImageCarousel.vue'
import LoadingSpinner from '@/components/LoadingSpinner.vue'
import CyberCursor from '@/components/CyberCursor.vue'

export default {
  name: 'Home',
  components: {
    ImageCarousel,
    LoadingSpinner,
    CyberCursor
  }
}
</script>

<style scoped>
.home {
  width: 100%;
  max-height: 100vh;
  overflow: scroll;
  position: relative;
}

.loading-section {
  position: absolute;
  bottom: 60px;
  right: 40px;
  z-index: 10;
}
</style>
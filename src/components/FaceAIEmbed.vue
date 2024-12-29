<template>
  <div class="faceai-embed">
    <div v-if="loading" class="loading">
      加载中...
    </div>
    <div v-else class="content">
      <!-- 这里可以放置从 FaceAI API 获取的内容 -->
      <div class="demo-container">
        <h2>Face AI Demo</h2>
        <div class="upload-section">
          <input 
            type="file" 
            @change="handleFileUpload" 
            accept="image/*"
            ref="fileInput"
          >
          <div class="preview" v-if="previewUrl">
            <img :src="previewUrl" alt="Preview">
          </div>
          <div class="results" v-if="results">
            <pre>{{ results }}</pre>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FaceAIEmbed',
  data() {
    return {
      loading: false,
      previewUrl: null,
      results: null
    }
  },
  methods: {
    async handleFileUpload(event) {
      const file = event.target.files[0];
      if (!file) return;

    
      this.previewUrl = URL.createObjectURL(file);
      

      try {
        this.loading = true;
        const formData = new FormData();
        formData.append('image', file);

        
        const response = await fetch('https://api.faceai.uno/analyze', {
          method: 'POST',
          body: formData
        });

        const data = await response.json();
        this.results = data;
      } catch (error) {
        console.error('Error:', error);
        this.results = { error: 'Processing failed, please try again' };
      } finally {
        this.loading = false;
      }
    }
  }
}
</script>

<style scoped>
.faceai-embed {
  width: 100%;
  min-height: 600px;
  background: #fff;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.demo-container {
  max-width: 800px;
  margin: 0 auto;
  text-align: center;
}

.upload-section {
  margin: 20px 0;
  padding: 20px;
  border: 2px dashed #ccc;
  border-radius: 8px;
}

.preview {
  margin: 20px 0;
}

.preview img {
  max-width: 100%;
  max-height: 400px;
  border-radius: 8px;
}

.results {
  margin-top: 20px;
  padding: 15px;
  background: #f5f5f5;
  border-radius: 8px;
  text-align: left;
}

.loading {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 200px;
  font-size: 1.2em;
  color: #666;
}
</style> 
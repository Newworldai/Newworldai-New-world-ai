<template>
  <main>
    <div class="title _1">
      <img src="@/assets/logo.png" alt="Logo" class="logo" />
      New World AI
    </div>
    <div class="title _2">Exploring the Future</div>
    <div class="title _3">Learn More</div>

    <ul class='slider'>
      <li v-for="(item, index) in items" 
          :key="item.title" 
          class='item'
          :style="{ backgroundImage: `url('${item.image}')` }">
        <div class='content' v-if="index === currentIndex || index === 1">
          <h2 class='title'>{{ item.title }}</h2>
          <p class='description'>{{ item.description }}</p>
          <button>Discover More</button>
        </div>
        <div v-else-if="index !== 1" class="locked-overlay">
          <ion-icon v-if="!item.isFullscreen" name="lock-closed-outline" class="lock-icon"></ion-icon>
        </div>
      </li>
    </ul>

    <nav class='nav'>
      <ion-icon class='btn prev' name="arrow-back-outline" @click="prevSlide"></ion-icon>
      <ion-icon class='btn next' name="arrow-forward-outline" @click="nextSlide" ref="nextButton"></ion-icon>
    </nav>

    <div class="chat-wrapper" :class="{ open: isChatOpen }">
      <Chat v-if="isChatOpen" />
    </div>

    <div class="chat-trigger" @click="openChat" v-show="!isChatOpen">
      <svg class="chat-icon" viewBox="0 0 24 24" width="30" height="30">
        <path class="icon-path" d="M20 2H4c-1.1 0-2 .9-2 2v18l4-4h14c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2z" />
        
        <line class="icon-line" x1="6" y1="8" x2="18" y2="8" />
        <line class="icon-line" x1="6" y1="12" x2="14" y2="12" />
        
        <path class="neon-glow" d="M20 2H4c-1.1 0-2 .9-2 2v18l4-4h14c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2z" />
      </svg>
    </div>

    <footer class="site-footer">
      <div class="social-links">
        <!-- X (Twitter) Official SVG -->
        <a href="https://x.com/faceai_sol" target="_blank" class="social-link">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" width="40" height="40">
            <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"></path>
          </svg>
        </a>

        <!-- GitHub Official Icon -->
        <a href="https://github.com/Newworldai/New-world-ai/tree/main" target="_blank" class="social-link">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" width="40" height="40">
            <path d="M12 0.296875C5.37187 0.296875 0 5.67188 0 12.2969C0 17.4969 3.43875 21.9062 8.2075 23.4425C8.8075 23.5525 9.0475 23.18 9.0475 22.8575C9.0475 22.575 9.0375 21.705 9.0375 20.8312C5.6725 21.5538 4.9675 19.685 4.9675 19.685C4.4275 18.3662 3.6475 18.0075 3.6475 18.0075C2.5575 17.2862 3.735 17.3025 3.735 17.3025C4.9375 17.39 5.56 18.5075 5.56 18.5075C6.6525 20.3325 8.41 19.8438 9.09 19.56C9.2075 18.7725 9.5075 18.285 9.8425 18.0075C7.1675 17.7325 4.3775 16.6725 4.3775 11.9925C4.3775 10.64 4.865 9.55 5.6575 8.695C5.5325 8.415 5.0925 7.125 5.7725 5.5075C5.7725 5.5075 6.8025 5.2025 9.0375 6.7225C10.0275 6.435 11.0725 6.29375 12.1175 6.2875C13.1625 6.29375 14.2075 6.435 15.1975 6.7225C17.43 5.2025 18.46 5.5075 18.46 5.5075C19.14 7.125 18.7 8.415 18.575 8.695C19.3675 9.55 19.855 10.64 19.855 11.9925C19.855 16.685 17.0575 17.725 14.3675 18.0075C14.7925 18.3575 15.175 19.0725 15.175 20.155C15.175 21.755 15.1575 22.995 15.1575 22.8575C15.1575 23.18 15.3975 23.5575 16.0075 23.4425C20.7675 21.9062 24.2063 17.4969 24.2063 12.2969C24.2063 5.67188 18.8344 0.296875 12 0.296875Z"></path>
          </svg>
        </a>

        <!-- GitBook Official Icon -->
        <a href="https://github.com/Newworldai/New-world-ai/blob/main/README.md" target="_blank" class="social-link">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" width="40" height="40">
            <path d="M3 3v18h12l6-6V3H3zm14 11v5h-5V15h5zm-2-8v3H5V6h10z"></path>
          </svg>
        </a>
      </div>
    </footer>
  </main>
</template>

<script>
import Chat from './chat.vue'

export default {
  name: 'ImageCarousel',
  components: {
    Chat,
  },
  data() {
    return {
      items: [
        {
          image: 'https://cdn.mos.cms.futurecdn.net/dP3N4qnEZ4tCTCLq59iysd.jpg',
          title: 'AI Dawn: The Beginning of a New Era',
          description: 'Embark on a journey to explore the dawn of artificial intelligence.',
          isFullscreen: false
        },
        {
          image: 'https://i.redd.it/tc0aqpv92pn21.jpg',
          title: 'Digital Bond: Connecting Worlds',
          description: 'In the digital age, new connections and bonds are forming across the globe.',
          isFullscreen: false
        },
        {
          image: 'https://wharferj.files.wordpress.com/2015/11/bio_north.jpg',
          title: 'Guardian of the Gate: Protecting the Future',
          description: 'As we stand on the brink of a new era, the guardians of the gate are tasked with ushering in a new age of intelligence. Explore the challenges and triumphs of those who protect the future, ensuring a safe and prosperous world for generations to come.',
          isFullscreen: false
        },
        {
          image: 'https://images7.alphacoders.com/878/878663.jpg',
          title: 'Trace of Humanity: The Human Touch in Technology',
          description: 'In a world driven by technological advancement, the traces of humanity remain ever-present. Discover how the human touch continues to influence and shape the progress of technology, preserving the essence of what makes us truly human.',
          isFullscreen: false
        },
        {
          image: 'https://theawesomer.com/photos/2017/07/simon_stalenhag_the_electric_state_6.jpg',
          title: 'Urban Evolution: Cities of the Future',
          description: 'Witness the evolution of urban landscapes as technology and nature blend seamlessly. Explore the cities of the future, where innovation and sustainability go hand in hand, creating vibrant and dynamic environments for all.',
          isFullscreen: false
        },
        {
          image: 'https://da.se/app/uploads/2015/09/simon-december1994.jpg',
          title: 'Journey of Change: Embracing the Unknown',
          description: 'Embark on a journey of transformation, where the unknown becomes an opportunity for growth and discovery. Embrace the changes that lie ahead, as we explore the uncharted territories of the future and unlock the potential within.',
          isFullscreen: false
        }
      ],
      isChatOpen: false,
      isFirstLoad: true,
      currentIndex: 0
    }
  },
  methods: {
    nextSlide() {
      const slider = document.querySelector('.slider');
      const items = document.querySelectorAll('.item');
      slider.appendChild(items[0]);
      this.isFirstLoad = false;
      this.currentIndex = (this.currentIndex + 1) % this.items.length;
    },
    prevSlide() {
      const slider = document.querySelector('.slider');
      const items = document.querySelectorAll('.item');
      slider.prepend(items[items.length - 1]);
      this.currentIndex = (this.currentIndex - 1 + this.items.length) % this.items.length;
    },
    openChat() {
      this.isChatOpen = true
    },
    closeChat() {
      this.isChatOpen = false
    },
    toggleChat() {
      this.isChatOpen = !this.isChatOpen
    },
  },
  mounted() {
    this.currentIndex = 0;
  }
}
</script>

<style scoped>
main {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  background: #000000;
  user-select: none;
}

.title {
  position: absolute;
  font-family: 'Courier New', Courier, monospace;
  color: rgb(0, 255, 255);
  font-size: 1.5rem;
  font-weight: bold;
  mix-blend-mode: normal;
  z-index: 2;
}

.title._1 { top: 20px; left: 30px; }
.title._2 { bottom: 20px; left: 30px; }
.title._3 { bottom: 20px; right: 30px; cursor: pointer; }

.slider {
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
  list-style: none;
  position: relative;
}

.item {
  width: 200px;
  height: 300px;
  list-style-type: none;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  z-index: 1;
  background-position: center;
  background-size: cover;
  border-radius: 20px;
  transition: transform 0.1s, left 0.75s, top 0.75s, width 0.75s, height 0.75s;
  border: 2px solid rgba(0, 255, 255, 0.5);
  box-shadow: 0 0 15px rgba(0, 255, 255, 0.5),
              0 0 30px rgba(0, 255, 255, 0.3) inset;
}

.item:nth-child(1), 
.item:nth-child(2) {
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  transform: none;
  border-radius: 0;
  box-shadow: none;
  opacity: 1;
  background-size: cover;
  background-position: center;
}

.item:nth-child(3) { left: 50%; }
.item:nth-child(4) { left: calc(50% + 220px); }
.item:nth-child(5) { left: calc(50% + 440px); }
.item:nth-child(6) { left: calc(50% + 660px); opacity: 0; }

.content {
  width: min(30vw,400px);
  position: absolute;
  top: 50%;
  left: 3rem;
  transform: translateY(-50%);
  font: bold 1.3rem 'Courier New', Courier, monospace;
  color: rgb(0, 255, 255);
  text-shadow: 0 3px 8px rgba(0, 255, 255, 0.5);
  opacity: 0;
  display: none;
  z-index: 2;
}

.content .title {
  font-family: 'Courier New', Courier, monospace;
  text-transform: uppercase;
  position: relative;
  top: 0;
  left: 0;
  font-weight: bolder;
}

.content .description {
  line-height: 1.7;
  margin: 1rem 0 1.5rem;
  font-size: 1.2rem;
  font-weight: bolder;
}

.content button {
  width: fit-content;
  background-color: rgba(0,0,0,0.1);
  color: rgb(0, 255, 255);
  border: 2px solid rgb(0, 255, 255);
  border-radius: 0.25rem;
  padding: 0.75rem;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: bolder;
}

.content button:hover {
  background-color: rgba(0, 255, 255, 0.1);
  transform: translateY(-2px);
}

.item:nth-of-type(2) .content {
  display: block;
  animation: show 0.75s ease-in-out 0.3s forwards;
}

@keyframes show {
  0% {
    filter: blur(5px);
    transform: translateY(calc(-50% + 75px));
    opacity: 0;
  }
  100% {
    opacity: 1;
    filter: blur(0);
    transform: translateY(-50%);
  }
}

.nav {
  position: absolute;
  bottom: 6rem;
  left: 50%;
  transform: translateX(-50%);
  z-index: 5;
  user-select: none;
}

.nav .btn {
  background-color: rgba(0, 255, 255, 0.5);
  color: rgba(0,0,0,0.7);
  border: 2px solid rgba(0,0,0,0.6);
  margin: 0 0.25rem;
  padding: 0.75rem;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease;
}

.nav .btn:hover {
  background-color: rgba(0, 255, 255, 0.3);
  transform: scale(1.1);
}

@keyframes breathe {
  from {
    transform: scale(1);
    opacity: 0.8;
  }
  to {
    transform: scale(1.2);
    opacity: 1;
  }
}

.chat-wrapper {
  position: fixed;
  right: -420px;
  top: 50%;
  transform: translateY(-50%);
  transition: right 0.3s ease;
  z-index: 9999;
}

.chat-wrapper.open {
  right: 20px;
}

.chat-trigger {
  position: absolute;
  right: 30px;
  bottom: 250px;
  width: 60px;
  height: 60px;
  background: rgba(0, 0, 0, 0.7);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 0 15px rgba(0, 255, 255, 0.3);
  z-index: 1000;
}

.chat-trigger:hover {
  transform: scale(1.1);
  box-shadow: 0 0 25px rgba(0, 255, 255, 0.5);
}

.chat-trigger:active {
  transform: scale(0.95);
}

.chat-icon {
  fill: none;
  stroke-width: 1.5;
}

.icon-path {
  stroke: #0ff;
  fill: transparent;
  stroke-width: 1.5;
  filter: drop-shadow(0 0 2px #0ff);
}

.icon-line {
  stroke: #0ff;
  stroke-width: 1.5;
  stroke-linecap: round;
}

.neon-glow {
  stroke: #0ff;
  stroke-width: 0.5;
  opacity: 0.5;
  filter: blur(3px);
  animation: neon-pulse 2s infinite alternate;
}

@keyframes neon-pulse {
  from {
    filter: drop-shadow(0 0 2px #0ff) 
            drop-shadow(0 0 4px #0ff);
    opacity: 0.5;
  }
  to {
    filter: drop-shadow(0 0 3px #0ff) 
            drop-shadow(0 0 6px #0ff) 
            drop-shadow(0 0 9px #0ff);
    opacity: 0.8;
  }
}

@media (prefers-color-scheme: dark) {
  .chat-trigger {
    background: rgba(20, 20, 20, 0.9);
  }
}

@media (width > 650px) and (width < 900px) {
  .content .title { font-size: 1rem; }
  .content .description { font-size: 0.7rem; }
  .content button { font-size: 0.7rem; }
  
  .item {
    width: 160px;
    height: 270px;
  }
  
  .item:nth-child(3) { left: 50%; }
  .item:nth-child(4) { left: calc(50% + 170px); }
  .item:nth-child(5) { left: calc(50% + 340px); }
  .item:nth-child(6) { left: calc(50% + 510px); opacity: 0; }
}

@media (width < 650px) {
  .content .title { font-size: 0.9rem; }
  .content .description { font-size: 0.65rem; }
  .content button { font-size: 0.7rem; }
  
  .item {
    width: 130px;
    height: 220px;
  }
  
  .item:nth-child(3) { left: 50%; }
  .item:nth-child(4) { left: calc(50% + 140px); }
  .item:nth-child(5) { left: calc(50% + 280px); }
  .item:nth-child(6) { left: calc(50% + 420px); opacity: 0; }
}

@media (max-width: 768px) {
  .content {
    width: 80vw;
    left: 1rem;
  }
  
  .item {
    width: 150px;
    height: 225px;
  }
  
  .item:nth-child(3) { left: 50%; }
  .item:nth-child(4) { left: calc(50% + 160px); }
  .item:nth-child(5) { left: calc(50% + 320px); }
  .item:nth-child(6) { left: calc(50% + 480px); opacity: 0; }
}

.site-footer {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  padding: 20px;
  text-align: center;
  z-index: 100;
}

.site-footer p {
  color: white;
  margin: 0;
  font-size: 14px;
}

.site-footer a {
  color: #0ff;
  text-decoration: none;
  transition: color 0.3s;
}

.site-footer a:hover {
  color: #00cccc;
}

.social-links {
  margin-top: 20px;
  display: flex;
  justify-content: center;
  gap: 20px;
}

.social-link {
  color: #fff;
  transition: transform 0.3s;
}

.social-link:hover {
  transform: scale(1.1);
}

.social-link svg {
  fill: currentColor;
}

@media (max-width: 768px) {
  .site-footer {
    padding: 15px;
  }
  
  .social-links {
    gap: 15px;
  }
  
  .social-link svg {
    width: 30px;
    height: 30px;
  }
}

.breathe {
  animation: breathe 0.8s infinite alternate;
}

@keyframes breathe {
  from {
    transform: scale(1);
    opacity: 0.8;
  }
  to {
    transform: scale(1.2);
    opacity: 1;
  }
}

.logo {
  width: 50px;
  height: auto;
  border-radius: 50%;
  margin-right: 5px;
  vertical-align: middle;
}

.locked-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 3;
  border-radius: 20px;
}

.lock-icon {
  color: rgb(0, 255, 255);
  font-size: 2rem;
}
</style> 
<template>
  <div>
    <div class="slider-container">
      <button @click="previousFact" class="slider-button">
        <i class="fas fa-chevron-left"></i>
      </button>
      <div class="fact-container">
        <transition name="fade">
          <p v-if="fact" :key="fact">{{ fact }}</p>
        </transition>
      </div>
      <button @click="nextFact" class="slider-button">
        <i class="fas fa-chevron-right"></i>
      </button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      facts: [],
      currentFactIndex: 0,
    };
  },
  computed: {
    fact() {
      return this.facts[this.currentFactIndex] || '';
    },
  },
  created() {
    this.getFacts();
    this.startSlideShow();
  },
  methods: {
    async getFacts() {
      try {
        const response = await axios.get('https://catfact.ninja/facts?limit=10');
        this.facts = response.data.data.map(f => f.fact);
      } catch (error) {
        console.error('Error fetching facts:', error);
      }
    },
    startSlideShow() {
      setInterval(this.nextFact, 7000);
    },
    nextFact() {
      this.currentFactIndex = (this.currentFactIndex + 1) % this.facts.length;
    },
    previousFact() {
      this.currentFactIndex =
        (this.currentFactIndex - 1 + this.facts.length) % this.facts.length;
    },
  },
};
</script>

<style scoped>
h1 {
  color: #333;
  text-align: center;
}

.slider-container {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  width: 100%;
  margin: 0 auto;
}

.fact-container {
  position: relative;
  padding: 20px;
  border-radius: 5px;
  text-align: center;
  height: 150px; 
  width: 80%;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
}

.fact-container::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: url('https://loremflickr.com/cache/resized/65535_53697834698_d421009e3b_h_800_600_nofilter.jpg'); /* تصویر با ابعاد مناسب */
  background-size: cover;
  background-position: center;
  filter: blur(4px); 
  z-index: 1;
  border-radius: 5px; 
}

.fact-container::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5); 
  z-index: 2;
  border-radius: 5px; 
}

.fact-container > * {
  position: relative;
  z-index: 3;
  color: #f0f0f0; 
}

.slider-button {
  background: none;
  border: none;
  font-size: 24px;
  cursor: pointer;
  user-select: none;
  color: white;
}

.slider-button i {
  font-size: 36px;
  color: #42b983;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>

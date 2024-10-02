<template>
  <div class="landing-page" @mousemove="updateMousePosition">
    <div class="content">
      <h1 class="modern-heading">
        {{ staticText }}<span class="typing">{{ typedText }}</span>
      </h1>
    </div>
    <div 
      class="portal" 
      :style="{ 
        left: mouseX + 'px', 
        top: mouseY + 'px',
      }"
    ></div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const staticText = "Hi, I'm ";
const fullText = "a Web Developer";
const typedText = ref('');
let currentIndex = 0;
let typingInterval;

const mouseX = ref(0);
const mouseY = ref(0);

const typeText = () => {
  if (currentIndex < fullText.length) {
    typedText.value += fullText[currentIndex];
    currentIndex++;
  } else {
    clearInterval(typingInterval);
    setTimeout(resetTyping, 2000);
  }
};

const resetTyping = () => {
  currentIndex = 0;
  typedText.value = '';
  typingInterval = setInterval(typeText, 100);
};

const updateMousePosition = (event) => {
  mouseX.value = event.clientX;
  mouseY.value = event.clientY;
};

onMounted(() => {
  typingInterval = setInterval(typeText, 100);
});

onUnmounted(() => {
  clearInterval(typingInterval);
});
</script>

<style scoped>
.landing-page {
  position: relative;
  height: 100vh;
  overflow: hidden;
  background: linear-gradient(135deg, #6e8efb, #a777e3);
}

.content {
  position: relative;
  z-index: 10;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
}

.modern-heading {
  font-size: 4rem;
  font-weight: bold;
  color: #ffffff;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
}

.typing {
  border-right: 0.1em solid #ffffff;
  animation: blink-caret 0.75s step-end infinite;
}

@keyframes blink-caret {
  from, to { border-color: transparent }
  50% { border-color: #ffffff }
}

.portal {
  position: fixed;
  width: 200px;
  height: 200px;
  border-radius: 50%;
  overflow: hidden;
  transform: translate(-50%, -50%);
  z-index: 5;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
  background-image: url('/images/IMG_9835.jpeg');
  background-size: cover;
  background-attachment: fixed;
}
</style>
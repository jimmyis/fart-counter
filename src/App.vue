<template>
  <div class="app">
    <header>
      <h1>Fart & Burp Tracker 3000</h1>
      <p>Your personal gas journal!</p>
    </header>

    <div class="counter">
      <h2>Farts: {{ fartCount }}</h2>
      <button @click="logFart" class="fart-button">Release the Gas!</button>
      <h2>Burps: {{ burpCount }}</h2>
      <button @click="logBurp" class="burp-button">Let Out a Burp!</button>
    </div>

    <div class="fart-cloud-container">
      <div
        v-for="(cloud, index) in fartClouds"
        :key="cloud.id"
        class="fart-cloud"
      >
        <span class="cloud-shape">{{ cloud.shape }}</span>
      </div>
    </div>

    <div class="burp-cloud-container">
      <div
        v-for="(cloud, index) in burpClouds"
        :key="cloud.id"
        class="burp-cloud"
      >
        <span class="cloud-shape">{{ cloud.shape }}</span>
      </div>
    </div>

    <footer>
      <p>Gas responsibly. — Fart & Burp Tracker Team</p>
    </footer>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  name: 'App',
  setup() {
    const fartCount = ref(0);
    const burpCount = ref(0);
    const fartClouds = ref([]);
    const burpClouds = ref([]);
    const cloudShapes = ['💨', '🌀', '🐉', '🌪️', '🔥', '✨', '👾'];

    onMounted(() => {
      const storedFartCount = localStorage.getItem('fartCount');
      if (storedFartCount) {
        fartCount.value = parseInt(storedFartCount);
      }
      const storedBurpCount = localStorage.getItem('burpCount');
      if (storedBurpCount) {
        burpCount.value = parseInt(storedBurpCount);
      }
    });

    const logFart = () => {
      fartCount.value++;
      localStorage.setItem('fartCount', fartCount.value);

      const randomShape = cloudShapes[Math.floor(Math.random() * cloudShapes.length)];
      fartClouds.value.push({ id: Date.now(), shape: randomShape });

      setTimeout(() => {
        fartClouds.value.shift();
      }, 1500); // Adjusted animation duration
    };

    const logBurp = () => {
      burpCount.value++;
      localStorage.setItem('burpCount', burpCount.value);

      const randomShape = cloudShapes[Math.floor(Math.random() * cloudShapes.length)];
      burpClouds.value.push({ id: Date.now(), shape: randomShape });

      setTimeout(() => {
        burpClouds.value.shift();
      }, 1500); // Adjusted animation duration
    };

    return {
      fartCount,
      burpCount,
      fartClouds,
      burpClouds,
      logFart,
      logBurp
    };
  }
};
</script>

<style scoped>
body {
  font-family: 'Comic Sans MS', cursive, sans-serif;
  text-align: center;
  background: linear-gradient(135deg, #ff9a9e, #fad0c4);
  color: #333;
  margin: 0;
}

header {
  padding: 20px;
  background-color: #ff5722;
  color: #fff;
  border-bottom: 5px dashed #ffcc00;
}

h1 {
  font-size: 3rem;
}

p {
  font-size: 1.2rem;
  font-style: italic;
}

.counter {
  margin: 30px;
  padding: 20px;
  background: radial-gradient(circle, #ffffff, #e1bee7);
  border: 3px dotted #7e57c2;
  border-radius: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

button.fart-button, button.burp-button {
  padding: 15px 30px;
  font-size: 1.5rem;
  font-weight: bold;
  color: #fff;
  background: linear-gradient(to right, #ff5722, #ff9800);
  border: none;
  border-radius: 10px;
  cursor: pointer;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.3);
  transition: transform 0.1s ease-in-out;
  margin: 10px;
}

button.fart-button:hover, button.burp-button:hover {
  background: linear-gradient(to right, #ff9800, #ff5722);
  transform: scale(1.1);
}

button.fart-button:active, button.burp-button:active {
  transform: scale(0.9);
}

footer {
  margin-top: 40px;
  font-size: 1.1rem;
  color: #444;
}

.fart-cloud-container, .burp-cloud-container {
  position: absolute;
  bottom: 50px;
  left: 50%;
  transform: translateX(-50%);
  pointer-events: none;
}

.fart-cloud {
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%) scale(1);
  font-size: 3rem;
  opacity: 1;
  animation: splashFartAnimation 1.5s ease-out forwards;
}

.burp-cloud {
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%) scale(1);
  font-size: 3rem;
  opacity: 1;
  animation: splashBurpAnimation 1.5s ease-out forwards;
}

@keyframes splashFartAnimation {
  0% {
    transform: translateY(0) scale(1);
    opacity: 1;
  }
  30% {
    transform: translateY(-50px) scale(1.3);
    opacity: 0.8;
  }
  60% {
    transform: translateY(-150px) scale(1.6);
    opacity: 0.5;
  }
  100% {
    transform: translateY(-300px) scale(2);
    opacity: 0;
  }
}

@keyframes splashBurpAnimation {
  0% {
    transform: translateY(0) scale(1);
    opacity: 1;
  }
  30% {
    transform: translateY(-40px) scale(1.2);
    opacity: 0.8;
  }
  60% {
    transform: translateY(-120px) scale(1.5);
    opacity: 0.5;
  }
  100% {
    transform: translateY(-250px) scale(1.8);
    opacity: 0;
  }
}
</style>

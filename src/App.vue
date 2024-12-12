<template>
  <div class="app">
    <header>
      <h1>Fart Counter</h1>
      <p>Let's track your farts today!</p>
    </header>

    <div class="counter">
      <h2>{{ fartCount }}</h2>
      <button @click="logFart">Log a Fart</button>
    </div>

    <div class="fart-cloud-container">
      <!-- Dynamically added fart clouds will go here -->
      <div v-for="(cloud, index) in fartClouds" :key="index" class="fart-cloud"></div>
    </div>

    <footer>
      <p>Keep calm and fart on!</p>
    </footer>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  name: 'App',
  setup() {
    const fartCount = ref(0);
    const fartClouds = ref([]); // Array to hold fart clouds for animation

    // Load fart count from localStorage
    onMounted(() => {
      const storedCount = localStorage.getItem('fartCount');
      if (storedCount) {
        fartCount.value = parseInt(storedCount);
      }
    });

    const logFart = () => {
      fartCount.value++;
      localStorage.setItem('fartCount', fartCount.value);

      // Add a fart cloud to the fartClouds array
      fartClouds.value.push({ id: Date.now() }); // Unique ID based on timestamp

      // Trigger the animation and remove the fart cloud after animation duration
      setTimeout(() => {
        fartClouds.value.shift(); // Remove the cloud from the array after animation
      }, 2000); // 2 seconds duration (matching animation time)
    };

    return {
      fartCount,
      fartClouds,
      logFart
    };
  }
};
</script>

<style scoped>
body {
  font-family: 'Arial', sans-serif;
  text-align: center;
  background-color: #f7f9fc;
  color: #333;
}

header {
  padding: 20px;
  background-color: #ffcc00;
  border-radius: 10px;
}

h1 {
  font-size: 2.5rem;
  color: #fff;
}

p {
  font-size: 1.1rem;
}

.counter {
  margin: 40px;
  padding: 20px;
  background-color: #e0f7fa;
  border-radius: 10px;
}

button {
  padding: 10px 20px;
  font-size: 1.2rem;
  background-color: #ff5722;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #e64a19;
}

footer {
  margin-top: 50px;
  font-size: 1.1rem;
  color: #777;
}

/* Fart cloud animation styles */
.fart-cloud-container {
  position: absolute;
  bottom: 50px; /* Start from the bottom of the screen */
  left: 50%;
  transform: translateX(-50%);
  pointer-events: none; /* Prevent the clouds from interfering with button clicks */
  z-index: 1;
}

.fart-cloud {
  position: absolute;
  bottom: 0;
  left: 50%;
  width: 100px;
  height: 100px;
  background-color: rgba(255, 255, 255, 0.7);
  border-radius: 50%;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  animation: fartAnimation 2s ease-out forwards;
  transform: translateX(-50%);
}

/* Fart cloud animation */
@keyframes fartAnimation {
  0% {
    transform: translateY(0) scale(1);
    opacity: 1;
  }
  50% {
    transform: translateY(-100px) scale(1.2);
    opacity: 0.8;
  }
  100% {
    transform: translateY(-200px) scale(1.5);
    opacity: 0;
  }
}
</style>

<script setup>
import PWADocumentation from './components/PWADocumentation.vue';
</script>

<template>
  <div class="app">
    <header class="app-header">
      <h1>PWA Release Documentation (2025)</h1>
      <p class="subtitle">Comprehensive guide for publishing PWAs to major app stores</p>
    </header>
    
    <main class="app-content">
      <PWADocumentation />
    </main>
    
    <footer class="app-footer">
      <p>© 2025 PWA Release Guide | Built with Vue.js</p>
    </footer>
  </div>
</template>

<style>
:root {
  --primary-color: #3498db;
  --secondary-color: #2c3e50;
  --background-color: #f5f7fa;
  --text-color: #333;
  --border-radius: 8px;
  --box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  --transition: all 0.3s ease;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  line-height: 1.6;
  color: var(--text-color);
  background-color: var(--background-color);
  min-height: 100vh;
}

.app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.app-header {
  background: linear-gradient(135deg, var(--primary-color), #9b59b6);
  color: white;
  text-align: center;
  padding: 2rem 1rem;
  margin-bottom: 2rem;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.app-header h1 {
  font-size: 2.5rem;
  margin-bottom: 0.5rem;
  font-weight: 600;
  text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.2);
}

.subtitle {
  font-size: 1.1rem;
  opacity: 0.9;
  max-width: 800px;
  margin: 0 auto;
}

.app-content {
  flex: 1;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 1rem 2rem;
}

.app-footer {
  text-align: center;
  padding: 1.5rem;
  background-color: var(--secondary-color);
  color: white;
  font-size: 0.9rem;
  margin-top: auto;
}

/* Smooth scrolling */
html {
  scroll-behavior: smooth;
}

/* Responsive Design */
@media (max-width: 768px) {
  .app-header h1 {
    font-size: 2rem;
  }
  
  .subtitle {
    font-size: 1rem;
  }
  
  .app-content {
    padding: 0 0.5rem 1.5rem;
  }
}
</style>

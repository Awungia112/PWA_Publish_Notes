<template>
  <div class="documentation-container">
    <header class="documentation-header">
      <h1>PWA Deployment Documentation</h1>
      <p class="subtitle">Comprehensive guide for deploying Progressive Web Apps to app stores</p>
    </header>

    <div class="documentation-grid">
      <aside class="sidebar">
        <div class="toc">
          <h2>Table of Contents</h2>
          <ul>
            <li><a href="#overview">Overview</a></li>
            <li><a href="#quick-start">Quick Start</a></li>
            <li><a href="#deployment-guide">Deployment Guide</a></li>
            <li><a href="#pwa-documentation">PWA Documentation</a></li>
          </ul>
        </div>
      </aside>

      <main class="content">
        <section id="overview" class="section">
          <h2>Overview</h2>
          <p>Welcome to the PWA Deployment Documentation. This guide provides comprehensive information about deploying Progressive Web Apps to major app stores, including Google Play and Apple App Store.</p>
        </section>

        <section id="quick-start" class="section">
          <h2>Quick Start</h2>
          <div class="quick-start-grid">
            <div class="quick-start-card">
              <h3>Google Play</h3>
              <ol>
                <li>Generate TWA with Bubblewrap</li>
                <li>Sign the APK/AAB</li>
                <li>Upload to Play Console</li>
              </ol>
            </div>
            <div class="quick-start-card">
              <h3>App Store</h3>
              <ol>
                <li>Create WKWebView wrapper</li>
                <li>Configure in Xcode</li>
                <li>Submit via App Store Connect</li>
              </ol>
            </div>
          </div>
        </section>

        <section id="deployment-guide" class="section">
          <h2>Deployment Guide</h2>
          <DeploymentGuide />
        </section>

        <section id="pwa-documentation" class="section">
          <h2>PWA Documentation</h2>
          <PWADocumentation />
        </section>
      </main>
    </div>
  </div>
</template>

<script>
import DeploymentGuide from '@/components/DeploymentGuide.vue';
import PWADocumentation from '@/components/PWADocumentation.vue';

export default {
  name: 'Documentation',
  components: {
    DeploymentGuide,
    PWADocumentation
  }
}
</script>

<style scoped>
.documentation-container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 2rem;
}

.documentation-header {
  text-align: center;
  margin-bottom: 3rem;
}

.documentation-header h1 {
  font-size: 2.5rem;
  color: #2c3e50;
  margin-bottom: 0.5rem;
}

.subtitle {
  font-size: 1.2rem;
  color: #666;
}

.documentation-grid {
  display: grid;
  grid-template-columns: 300px 1fr;
  gap: 2rem;
}

.sidebar {
  position: sticky;
  top: 2rem;
  align-self: start;
}

.toc {
  background: #f8f9fa;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
}

.toc h2 {
  font-size: 1.2rem;
  margin-top: 0;
  margin-bottom: 1rem;
  color: #2c3e50;
}

.toc ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.toc li {
  margin-bottom: 0.5rem;
}

.toc a {
  color: #4a6cf7;
  text-decoration: none;
  transition: color 0.2s;
}

.toc a:hover {
  color: #2a4bd7;
  text-decoration: underline;
}

.section {
  margin-bottom: 3rem;
  background: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.05);
}

.section h2 {
  color: #2c3e50;
  border-bottom: 2px solid #f0f0f0;
  padding-bottom: 0.5rem;
  margin-top: 0;
}

.quick-start-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.5rem;
  margin-top: 1.5rem;
}

.quick-start-card {
  background: #f8f9fa;
  padding: 1.5rem;
  border-radius: 8px;
  border-left: 4px solid #4a6cf7;
}

.quick-start-card h3 {
  margin-top: 0;
  color: #2c3e50;
}

.quick-start-card ol {
  padding-left: 1.2rem;
}

.quick-start-card li {
  margin-bottom: 0.5rem;
}

/* Responsive adjustments */
@media (max-width: 1024px) {
  .documentation-grid {
    grid-template-columns: 1fr;
  }
  
  .sidebar {
    position: static;
    margin-bottom: 2rem;
  }
}

@media (max-width: 768px) {
  .documentation-container {
    padding: 1rem;
  }
  
  .quick-start-grid {
    grid-template-columns: 1fr;
  }
}
</style>
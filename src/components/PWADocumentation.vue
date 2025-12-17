<template>
  <div class="pwa-documentation">
    <h1 class="title">PWA Release Documentation (2025)</h1>
    
    <div class="section" v-for="(section, index) in sections" :key="index" :class="'section-' + index">
      <h2 class="section-title" @click="toggleSection(index)">
        {{ section.title }}
        <span class="toggle-icon">{{ section.isOpen ? '−' : '+' }}</span>
      </h2>
      
      <transition name="fade" mode="out-in">
        <div v-if="section.isOpen" class="section-content">
          <div v-html="section.content"></div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PWADocumentation',
  data() {
    return {
      sections: [
        {
          title: '1. Core Libraries and Tooling',
          isOpen: true,
          content: `
            <p>Publishing a Progressive Web App (PWA) to major stores requires wrapping it in a native shell that satisfies platform-specific requirements.</p>
            <div class="tool-grid">
              <div class="tool-card">
                <h3>PWABuilder</h3>
                <p>The primary cross-platform tool for generating store-ready packages for Android, iOS, and Microsoft Store.</p>
                <div class="tool-links">
                  <a href="https://www.pwabuilder.com/" target="_blank" rel="noopener">Website</a>
                  <a href="https://github.com/pwa-builder" target="_blank" rel="noopener">GitHub</a>
                </div>
              </div>
              
              <div class="tool-card">
                <h3>Bubblewrap CLI</h3>
                <p>The industry standard for Google Play. It converts your PWA into a TWA-based Android App Bundle (AAB).</p>
                <div class="tool-links">
                  <a href="https://github.com/GoogleChromeLabs/bubblewrap" target="_blank" rel="noopener">GitHub</a>
                  <a href="https://www.npmjs.com/package/@bubblewrap/cli" target="_blank" rel="noopener">npm</a>
                </div>
              </div>
              
              <div class="tool-card">
                <h3>Capacitor</h3>
                <p>A robust alternative to PWABuilder that provides a bridge for native device features (biometrics, camera) while maintaining a single codebase.</p>
                <div class="tool-links">
                  <a href="https://capacitorjs.com/" target="_blank" rel="noopener">Website</a>
                  <a href="https://github.com/ionic-team/capacitor" target="_blank" rel="noopener">GitHub</a>
                </div>
              </div>
              
              <div class="tool-card">
                <h3>Fastlane</h3>
                <p>Essential for automating the signing and submission of builds to both App Store Connect and Google Play Console.</p>
                <div class="tool-links">
                  <a href="https://fastlane.tools/" target="_blank" rel="noopener">Website</a>
                  <a href="https://github.com/fastlane/fastlane" target="_blank" rel="noopener">GitHub</a>
                </div>
              </div>
            </div>
          `
        },
        {
          title: '2. Setup and Build Procedures',
          isOpen: false,
          content: `
            <div class="platform-section">
              <div class="platform-card">
                <div class="platform-header google">
                  <h3>Google Play Store (Android)</h3>
                  <span class="platform-badge">TWA Required</span>
                </div>
                <div class="platform-content">
                  <h4>Protocol</h4>
                  <p>Uses Trusted Web Activity (TWA), which runs your PWA in a full-screen Chrome instance.</p>
                  
                  <h4>Verification</h4>
                  <p>You must host a <code>/.well-known/assetlinks.json</code> file on your web server to prove ownership of the domain.</p>
                  
                  <h4>Build Steps</h4>
                  <ol>
                    <li>Install Bubblewrap CLI: <code>npm install -g @bubblewrap/cli</code></li>
                    <li>Initialize project: <code>bubblewrap init --manifest=https://your-pwa-url/manifest.json</code></li>
                    <li>Build the AAB: <code>bubblewrap build</code></li>
                  </ol>
                  
                  <h4>Billing</h4>
                  <p>For digital goods, implement the <a href="https://developer.chrome.com/docs/android/trusted-web-activity/digital-asset-links/" target="_blank" rel="noopener">Digital Goods API</a> to integrate with Google Play Billing.</p>
                </div>
              </div>
              
              <div class="platform-card">
                <div class="platform-header apple">
                  <h3>Apple App Store (iOS)</h3>
                  <span class="platform-badge">WKWebView Required</span>
                </div>
                <div class="platform-content">
                  <h4>Requirements</h4>
                  <ul>
                    <li>macOS with Xcode 16+</li>
                    <li>iOS 18 SDK</li>
                    <li>Apple Developer Account ($99/year)</li>
                  </ul>
                  
                  <h4>Build Steps</h4>
                  <ol>
                    <li>Generate Xcode project using PWABuilder or Capacitor</li>
                    <li>Open in Xcode 16+</li>
                    <li>Configure signing certificates and provisioning profiles</li>
                    <li>Set up Push Notifications if needed</li>
                    <li>Archive and upload to App Store Connect</li>
                  </ol>
                  
                  <h4>App Store Guidelines</h4>
                  <ul>
                    <li>Must provide a native-like experience</li>
                    <li>Include proper app privacy details</li>
                    <li>Follow Human Interface Guidelines</li>
                  </ul>
                </div>
              </div>
            </div>
          `
        },
        {
          title: '3. CI/CD Integration',
          isOpen: false,
          content: `
            <div class="ci-cd-section">
              <h3>Automated Deployment Pipeline</h3>
              <p>Fully automate your build and deployment process using GitHub Actions or Codemagic.</p>
              
              <h4>GitHub Actions Setup</h4>
              <p>Example workflow configuration (<code>.github/workflows/deploy.yml</code>):</p>
              
              <pre><code>name: Deploy PWA to Stores

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  build-and-deploy:
    runs-on: \${{ matrixos }}
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest]
        include:
          - os: ubuntu-latest
            platform: android
          - os: macos-latest
            platform: ios
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Use Node.js 18
        uses: actions/setup-node@v3
        with:
          node-version: \'18\'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Build PWA
        run: npm run build
        
      - name: Android Build & Deploy
        if: matrix.platform == \'android\'
        uses: 1gravity/action-bubblewrap@v1
        with:
          manifest: \'https://your-pwa-url/manifest.json\'
          keyStore: \${{ secrets.ANDROID_KEYSTORE }}
          keyStorePassword: \${{ secrets.KEYSTORE_PASSWORD }}
          
      - name: iOS Build & Deploy
        if: matrix.platform == \'ios\'
        run: |
          cd ios
          fastlane beta
        env:
          FASTLANE_APPLE_APPLICATION_SPECIFIC_PASSWORD: \${{ secrets.FASTLANE_APPLE_APPLICATION_SPECIFIC_PASSWORD }}
          MATCH_PASSWORD: \${{ secrets.MATCH_PASSWORD }}</code></pre>

              <h4>Required Secrets</h4>
              <div class="secrets-grid">
                <div class="secret-card">
                  <h5>Android</h5>
                  <ul>
                    <li><code>ANDROID_KEYSTORE</code></li>
                    <li><code>KEYSTORE_PASSWORD</code></li>
                    <li><code>KEY_ALIAS</code></li>
                    <li><code>KEY_PASSWORD</code></li>
                  </ul>
                </div>
                
                <div class="secret-card">
                  <h5>iOS</h5>
                  <ul>
                    <li><code>APPLE_DEVELOPER_ACCOUNT</code></li>
                    <li><code>FASTLANE_APPLE_APPLICATION_SPECIFIC_PASSWORD</code></li>
                    <li><code>MATCH_PASSWORD</code></li>
                    <li><code>APP_STORE_CONNECT_API_KEY</code></li>
                  </ul>
                </div>
              </div>
              
              <h4>Testing & Quality Gates</h4>
              <ul>
                <li>Integrate Lighthouse CI for performance testing</li>
                <li>Set minimum performance score requirements (recommended: 80+)</li>
                <li>Run automated tests on real devices using BrowserStack or similar</li>
                <li>Validate PWA requirements before deployment</li>
              </ul>
            </div>
          `
        }
      ]
    };
  },
  methods: {
    toggleSection(index) {
      this.sections[index].isOpen = !this.sections[index].isOpen;
    }
  }
};
</script>

<style scoped>
.pwa-documentation {
  max-width: 1000px;
  margin: 0 auto;
  padding: 2rem 1rem;
  color: #333;
  line-height: 1.6;
}

.title {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 2rem;
  font-size: 2.2rem;
  background: linear-gradient(135deg, #3498db, #9b59b6);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  padding-bottom: 1rem;
  border-bottom: 2px solid #f0f0f0;
}

.section {
  background: white;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  margin-bottom: 1.5rem;
  overflow: hidden;
  transition: all 0.3s ease;
}

.section:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

.section-title {
  background: linear-gradient(135deg, #f5f7fa, #e0e6ed);
  color: #2c3e50;
  padding: 1.25rem 2rem;
  margin: 0;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: all 0.3s ease;
  font-size: 1.2rem;
  font-weight: 600;
  user-select: none;
}

.section-title:hover {
  background: linear-gradient(135deg, #e6f0ff, #d0e1ff);
}

.toggle-icon {
  font-size: 1.5rem;
  font-weight: bold;
  transition: transform 0.3s ease;
  width: 24px;
  text-align: center;
}

.section-content {
  padding: 1.5rem 2rem;
  background: white;
  border-top: 1px solid #eee;
  transition: all 0.3s ease;
}

/* Tool Grid */
.tool-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
  margin: 1.5rem 0;
}

.tool-card {
  background: #f9f9f9;
  border-radius: 8px;
  padding: 1.5rem;
  border: 1px solid #eee;
  transition: all 0.2s ease;
}

.tool-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.tool-card h3 {
  margin-top: 0;
  color: #2c3e50;
  font-size: 1.2rem;
}

.tool-card p {
  color: #555;
  margin: 0.75rem 0 1rem;
  font-size: 0.95rem;
  line-height: 1.5;
}

.tool-links {
  display: flex;
  gap: 0.75rem;
  margin-top: 1rem;
}

.tool-links a {
  display: inline-block;
  padding: 0.4rem 0.8rem;
  background: #3498db;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  font-size: 0.85rem;
  transition: background 0.2s ease;
}

.tool-links a:hover {
  background: #2980b9;
}

/* Platform Sections */
.platform-section {
  display: grid;
  grid-template-columns: 1fr;
  gap: 2rem;
  margin: 1.5rem 0;
}

.platform-card {
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.platform-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.12);
}

.platform-header {
  padding: 1rem 1.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.platform-header.google {
  background: linear-gradient(135deg, #4285f4, #34a853);
  color: white;
}

.platform-header.apple {
  background: linear-gradient(135deg, #000000, #434343);
  color: white;
}

.platform-header h3 {
  margin: 0;
  font-size: 1.2rem;
}

.platform-badge {
  background: rgba(255, 255, 255, 0.2);
  padding: 0.25rem 0.75rem;
  border-radius: 12px;
  font-size: 0.8rem;
  font-weight: 500;
}

.platform-content {
  padding: 1.5rem;
  background: white;
}

.platform-content h4 {
  color: #2c3e50;
  margin: 1.5rem 0 0.75rem;
  font-size: 1.1rem;
}

.platform-content h4:first-child {
  margin-top: 0;
}

/* CI/CD Section */
.ci-cd-section {
  margin: 1.5rem 0;
}

.secrets-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 1.5rem;
  margin: 1.5rem 0;
}

.secret-card {
  background: #f8f9fa;
  border-radius: 6px;
  padding: 1.25rem;
  border: 1px solid #e9ecef;
}

.secret-card h5 {
  margin-top: 0;
  color: #2c3e50;
  font-size: 1rem;
  padding-bottom: 0.5rem;
  border-bottom: 1px solid #e9ecef;
  margin-bottom: 1rem;
}

.secret-card ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.secret-card li {
  margin-bottom: 0.5rem;
  font-family: 'Courier New', Courier, monospace;
  font-size: 0.85rem;
  color: #2c3e50;
  word-break: break-all;
}

/* Code Blocks */
pre {
  background: #282c34;
  color: #abb2bf;
  padding: 1rem;
  border-radius: 6px;
  overflow-x: auto;
  margin: 1.5rem 0;
  font-size: 0.9rem;
  line-height: 1.5;
  font-family: 'Courier New', Courier, monospace;
}

code {
  background: #f0f2f5;
  padding: 0.2rem 0.4rem;
  border-radius: 4px;
  font-family: 'Courier New', Courier, monospace;
  font-size: 0.9em;
  color: #e74c3c;
}

pre code {
  background: transparent;
  color: inherit;
  padding: 0;
  font-size: inherit;
  font-family: inherit;
}

/* Animations */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}

/* Responsive Design */
@media (max-width: 768px) {
  .pwa-documentation {
    padding: 1rem;
  }
  
  .section-title {
    padding: 1rem;
    font-size: 1.1rem;
  }
  
  .section-content {
    padding: 1rem;
  }
  
  .tool-grid {
    grid-template-columns: 1fr;
  }
  
  .platform-section {
    grid-template-columns: 1fr;
  }
}

/* Print styles */
@media print {
  .pwa-documentation {
    padding: 0;
    max-width: 100%;
  }
  
  .section {
    break-inside: avoid;
    page-break-inside: avoid;
    box-shadow: none;
    border: 1px solid #eee;
  }
  
  .section-title {
    background: #f5f5f5 !important;
    color: #000 !important;
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }
  
  .toggle-icon {
    display: none;
  }
  
  .section-content {
    display: block !important;
  }
  
  a {
    text-decoration: none;
    color: inherit;
  }
  
  .tool-links {
    display: none;
  }
}
</style>
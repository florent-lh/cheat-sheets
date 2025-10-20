# PWA CheatSheet

## Table des Matières
1. [Concepts de Base](#concepts-de-base)
2. [Manifest Web App](#manifest-web-app)
3. [Service Workers](#service-workers)
4. [Stratégies de Cache](#stratégies-de-cache)
5. [APIs Essentielles](#apis-essentielles)
6. [Nouveautés 2024-2025](#nouveautés-2024-2025)
7. [Installation et Prompts](#installation-et-prompts)
8. [Performance et Optimisation](#performance-et-optimisation)
9. [Debugging et Outils](#debugging-et-outils)

---

## Concepts de Base

### Qu'est-ce qu'une PWA ?
Une Progressive Web App est une application web qui utilise des technologies modernes pour offrir une expérience similaire aux applications natives.

### Critères d'une PWA
- ✅ **HTTPS** obligatoire
- ✅ **Manifest** (manifest.json)
- ✅ **Service Worker** enregistré
- ✅ **Responsive** design
- ✅ **Installable** sur l'appareil

### Avantages
- Fonctionnement offline
- Installation sur l'écran d'accueil
- Notifications push
- Performances optimisées
- Mise à jour automatique
- Pas de store nécessaire

---

## Manifest Web App

### Structure de Base (manifest.json)

```json
{
  "name": "Mon Application PWA",
  "short_name": "MonApp",
  "description": "Description de mon application",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#2196F3",
  "orientation": "portrait",
  "scope": "/",
  "lang": "fr-FR",
  "dir": "ltr",
  "icons": [
    {
      "src": "/icons/icon-72x72.png",
      "sizes": "72x72",
      "type": "image/png",
      "purpose": "any"
    },
    {
      "src": "/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png",
      "purpose": "maskable"
    },
    {
      "src": "/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png",
      "purpose": "any maskable"
    }
  ]
}
```

### Propriétés du Manifest

#### Display Modes
```json
"display": "fullscreen"    // Plein écran
"display": "standalone"    // App indépendante (recommandé)
"display": "minimal-ui"    // UI minimale
"display": "browser"       // Navigateur standard
```

#### Display Override 
```json
"display_override": ["window-controls-overlay", "standalone"]
```

#### Categories
```json
"categories": ["productivity", "business", "social"]
```

#### Shortcuts (Raccourcis d'application)
```json
"shortcuts": [
  {
    "name": "Nouvelle tâche",
    "short_name": "Tâche",
    "description": "Créer une nouvelle tâche",
    "url": "/new-task",
    "icons": [{ "src": "/icons/task.png", "sizes": "192x192" }]
  }
]
```

#### Screenshots
```json
"screenshots": [
  {
    "src": "/screenshots/home.png",
    "sizes": "1280x720",
    "type": "image/png",
    "form_factor": "wide",
    "label": "Page d'accueil"
  },
  {
    "src": "/screenshots/mobile.png",
    "sizes": "750x1334",
    "type": "image/png",
    "form_factor": "narrow"
  }
]
```

#### Protocol Handlers
```json
"protocol_handlers": [
  {
    "protocol": "web+music",
    "url": "/player?track=%s"
  }
]
```

#### Share Target (Partage vers l'app)
```json
"share_target": {
  "action": "/share",
  "method": "POST",
  "enctype": "multipart/form-data",
  "params": {
    "title": "title",
    "text": "text",
    "url": "url",
    "files": [
      {
        "name": "media",
        "accept": ["image/*", "video/*"]
      }
    ]
  }
}
```

#### File Handlers
```json
"file_handlers": [
  {
    "action": "/open-file",
    "accept": {
      "image/*": [".png", ".jpg", ".jpeg"],
      "application/pdf": [".pdf"]
    }
  }
]
```

### Lien dans le HTML
```html
<link rel="manifest" href="/manifest.json">
<meta name="theme-color" content="#2196F3">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black">
<meta name="apple-mobile-web-app-title" content="MonApp">
<link rel="apple-touch-icon" href="/icons/icon-192x192.png">
```

---

## Service Workers

### Enregistrement du Service Worker

```javascript
// Dans votre fichier principal (main.js ou index.js)
if ('serviceWorker' in navigator) {
  window.addEventListener('load', async () => {
    try {
      const registration = await navigator.serviceWorker.register('/sw.js');
      console.log('SW enregistré:', registration.scope);
      
      // Vérifier les mises à jour
      registration.addEventListener('updatefound', () => {
        const newWorker = registration.installing;
        console.log('Nouvelle version disponible');
      });
    } catch (error) {
      console.error('Erreur SW:', error);
    }
  });
}
```

### Structure du Service Worker (sw.js)

```javascript
const CACHE_NAME = 'mon-app-v1';
const RUNTIME_CACHE = 'runtime-cache';

const PRECACHE_URLS = [
  '/',
  '/index.html',
  '/styles/main.css',
  '/scripts/main.js',
  '/images/logo.png',
  '/manifest.json'
];

// Installation
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then((cache) => cache.addAll(PRECACHE_URLS))
      .then(() => self.skipWaiting())
  );
});

// Activation
self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys().then((cacheNames) => {
      return Promise.all(
        cacheNames
          .filter((name) => name !== CACHE_NAME && name !== RUNTIME_CACHE)
          .map((name) => caches.delete(name))
      );
    }).then(() => self.clients.claim())
  );
});

// Interception des requêtes
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then((response) => {
        if (response) {
          return response;
        }
        return fetch(event.request).then((response) => {
          if (!response || response.status !== 200) {
            return response;
          }
          const responseToCache = response.clone();
          caches.open(RUNTIME_CACHE)
            .then((cache) => cache.put(event.request, responseToCache));
          return response;
        });
      })
  );
});
```

### Communication avec le Service Worker

```javascript
// Depuis la page
navigator.serviceWorker.controller?.postMessage({
  type: 'SKIP_WAITING'
});

// Dans le Service Worker
self.addEventListener('message', (event) => {
  if (event.data.type === 'SKIP_WAITING') {
    self.skipWaiting();
  }
});
```

---

## Stratégies de Cache

### 1. Cache First (Cache d'abord)
```javascript
// Idéal pour: assets statiques (CSS, JS, images)
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then((cached) => cached || fetch(event.request))
  );
});
```

### 2. Network First (Réseau d'abord)
```javascript
// Idéal pour: contenu dynamique, API
self.addEventListener('fetch', (event) => {
  event.respondWith(
    fetch(event.request)
      .then((response) => {
        const clone = response.clone();
        caches.open(CACHE_NAME)
          .then((cache) => cache.put(event.request, clone));
        return response;
      })
      .catch(() => caches.match(event.request))
  );
});
```

### 3. Stale While Revalidate
```javascript
// Idéal pour: équilibre fraîcheur/performance
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.open(CACHE_NAME).then((cache) => {
      return cache.match(event.request).then((cached) => {
        const fetchPromise = fetch(event.request).then((response) => {
          cache.put(event.request, response.clone());
          return response;
        });
        return cached || fetchPromise;
      });
    })
  );
});
```

### 4. Network Only
```javascript
// Idéal pour: requêtes qui ne doivent jamais être mises en cache
self.addEventListener('fetch', (event) => {
  event.respondWith(fetch(event.request));
});
```

### 5. Cache Only
```javascript
// Idéal pour: application 100% offline
self.addEventListener('fetch', (event) => {
  event.respondWith(caches.match(event.request));
});
```

---

## APIs Essentielles

### 1. Notifications Push

#### Demander la permission
```javascript
async function requestNotificationPermission() {
  if (!('Notification' in window)) {
    console.log('Notifications non supportées');
    return;
  }
  
  const permission = await Notification.requestPermission();
  if (permission === 'granted') {
    console.log('Permission accordée');
  }
}
```

#### Afficher une notification
```javascript
// Depuis la page
if (Notification.permission === 'granted') {
  new Notification('Titre', {
    body: 'Message de la notification',
    icon: '/icon.png',
    badge: '/badge.png',
    tag: 'unique-id',
    vibrate: [200, 100, 200]
  });
}

// Depuis le Service Worker
self.registration.showNotification('Titre', {
  body: 'Message',
  icon: '/icon.png',
  badge: '/badge.png',
  actions: [
    { action: 'accept', title: 'Accepter', icon: '/accept.png' },
    { action: 'decline', title: 'Refuser', icon: '/decline.png' }
  ],
  data: { url: '/details' }
});
```

#### Gérer les clics sur les notifications
```javascript
self.addEventListener('notificationclick', (event) => {
  event.notification.close();
  
  if (event.action === 'accept') {
    // Action accepter
  } else {
    // Ouvrir l'application
    event.waitUntil(
      clients.openWindow(event.notification.data.url)
    );
  }
});
```

### 2. Background Sync

```javascript
// Enregistrer une synchronisation
navigator.serviceWorker.ready.then((registration) => {
  return registration.sync.register('sync-tag');
});

// Dans le Service Worker
self.addEventListener('sync', (event) => {
  if (event.tag === 'sync-tag') {
    event.waitUntil(
      // Synchroniser les données
      syncData()
    );
  }
});
```

### 3. Periodic Background Sync 

```javascript
// Demander la permission et enregistrer
const status = await navigator.permissions.query({
  name: 'periodic-background-sync'
});

if (status.state === 'granted') {
  const registration = await navigator.serviceWorker.ready;
  await registration.periodicSync.register('content-sync', {
    minInterval: 24 * 60 * 60 * 1000 // 24 heures
  });
}

// Dans le Service Worker
self.addEventListener('periodicsync', (event) => {
  if (event.tag === 'content-sync') {
    event.waitUntil(updateContent());
  }
});
```

### 4. Background Fetch 

```javascript
// Pour les téléchargements longs
const registration = await navigator.serviceWorker.ready;
const bgFetch = await registration.backgroundFetch.fetch(
  'my-download',
  ['/large-file.mp4'],
  {
    title: 'Téléchargement en cours',
    icons: [{ src: '/icon.png', sizes: '192x192' }],
    downloadTotal: 60 * 1024 * 1024 // 60 MB
  }
);

// Suivre la progression
bgFetch.addEventListener('progress', () => {
  console.log(`Téléchargé: ${bgFetch.downloaded}/${bgFetch.downloadTotal}`);
});
```

### 5. Web Share API

```javascript
async function shareContent() {
  if (navigator.share) {
    try {
      await navigator.share({
        title: 'Mon titre',
        text: 'Description',
        url: 'https://example.com',
        files: [new File(['content'], 'file.txt', { type: 'text/plain' })]
      });
    } catch (error) {
      console.log('Partage annulé');
    }
  }
}
```

### 6. Badging API 

```javascript
// Définir un badge
if ('setAppBadge' in navigator) {
  navigator.setAppBadge(5); // Affiche "5"
  // ou
  navigator.setAppBadge(); // Affiche juste un point
}

// Effacer le badge
navigator.clearAppBadge();
```

### 7. File System Access API 

```javascript
// Ouvrir un fichier
const [fileHandle] = await window.showOpenFilePicker({
  types: [{
    description: 'Images',
    accept: { 'image/*': ['.png', '.jpg', '.jpeg'] }
  }]
});
const file = await fileHandle.getFile();

// Sauvegarder un fichier
const handle = await window.showSaveFilePicker({
  suggestedName: 'mon-fichier.txt'
});
const writable = await handle.createWritable();
await writable.write('Contenu du fichier');
await writable.close();
```

### 8. Screen Wake Lock

```javascript
let wakeLock = null;

async function requestWakeLock() {
  try {
    wakeLock = await navigator.wakeLock.request('screen');
    console.log('Écran maintenu actif');
    
    wakeLock.addEventListener('release', () => {
      console.log('Wake Lock libéré');
    });
  } catch (err) {
    console.error('Erreur Wake Lock:', err);
  }
}

// Libérer le wake lock
if (wakeLock) {
  wakeLock.release();
}
```

### 9. Web Bluetooth & NFC 

```javascript
// Bluetooth
async function connectBluetooth() {
  const device = await navigator.bluetooth.requestDevice({
    filters: [{ services: ['battery_service'] }]
  });
  const server = await device.gatt.connect();
  // ...
}

// NFC
async function readNFC() {
  if ('NDEFReader' in window) {
    const reader = new NDEFReader();
    await reader.scan();
    reader.onreading = (event) => {
      console.log('Tag NFC détecté:', event.serialNumber);
    };
  }
}
```

### 10. Contact Picker API 

```javascript
async function selectContact() {
  const props = ['name', 'email', 'tel'];
  const opts = { multiple: false };
  
  try {
    const contacts = await navigator.contacts.select(props, opts);
    console.log(contacts[0].name);
  } catch (error) {
    console.log('Sélection annulée');
  }
}
```

---

## Nouveautés 2024-2025

### 1. Window Controls Overlay (WCO)

```javascript
// Permettre le drag de la fenêtre sur desktop
if ('windowControlsOverlay' in navigator) {
  navigator.windowControlsOverlay.visible; // true/false
  
  navigator.windowControlsOverlay.addEventListener('geometrychange', (event) => {
    const { x, y, width, height } = event.titlebarAreaRect;
    // Ajuster l'UI en fonction
  });
}
```

```css
/* CSS pour WCO */
.title-bar {
  app-region: drag;
  -webkit-app-region: drag;
}

.button {
  app-region: no-drag;
  -webkit-app-region: no-drag;
}
```

### 2. Declarative Link Capturing 

```json
// Dans manifest.json
{
  "capture_links": "new-client",
  "scope_extensions": [
    { "origin": "*.example.com" }
  ]
}
```

### 3. Launch Handler 

```json
// Dans manifest.json
{
  "launch_handler": {
    "client_mode": ["navigate-existing", "auto"]
  }
}
```

```javascript
// Gérer les lancements
window.launchQueue.setConsumer((launchParams) => {
  if (launchParams.targetURL) {
    console.log('Lancé avec URL:', launchParams.targetURL);
    // Naviguer vers l'URL ou traiter les paramètres
  }
});
```

### 4. Manifest ID 

```json
{
  "id": "/",
  "start_url": "/",
  "scope": "/"
}
```

### 5. Permissions Policy

```html
<iframe src="..." allow="camera; microphone; geolocation"></iframe>
```

```javascript
// Vérifier les permissions
const result = await navigator.permissions.query({ name: 'camera' });
console.log(result.state); // 'granted', 'denied', 'prompt'

result.addEventListener('change', () => {
  console.log('Permission changée:', result.state);
});
```

### 6. Service Worker Static Routing API 

```javascript
// Optimisation des performances
addEventListener('install', (event) => {
  event.addRoutes([
    {
      condition: {
        urlPattern: new URLPattern({ pathname: '/static/*' })
      },
      source: 'cache'
    },
    {
      condition: {
        urlPattern: new URLPattern({ pathname: '/api/*' })
      },
      source: 'network'
    }
  ]);
});
```

---

## Installation et Prompts

### Détecter si l'app est installée

```javascript
// Vérifier le mode d'affichage
if (window.matchMedia('(display-mode: standalone)').matches) {
  console.log('App installée et lancée en mode standalone');
}

// Ou vérifier via navigator
if (navigator.standalone) { // iOS
  console.log('App installée sur iOS');
}
```

### Prompt d'installation personnalisé

```javascript
let deferredPrompt;

window.addEventListener('beforeinstallprompt', (e) => {
  // Empêcher le prompt automatique
  e.preventDefault();
  deferredPrompt = e;
  
  // Afficher votre bouton d'installation
  showInstallButton();
});

async function installApp() {
  if (!deferredPrompt) return;
  
  // Afficher le prompt
  deferredPrompt.prompt();
  
  // Attendre la réponse de l'utilisateur
  const { outcome } = await deferredPrompt.userChoice;
  
  if (outcome === 'accepted') {
    console.log('App installée');
  } else {
    console.log('Installation refusée');
  }
  
  deferredPrompt = null;
}

// Détecter l'installation réussie
window.addEventListener('appinstalled', () => {
  console.log('PWA installée avec succès');
  hideInstallButton();
});
```

### Détection des fonctionnalités

```javascript
const features = {
  serviceWorker: 'serviceWorker' in navigator,
  notifications: 'Notification' in window,
  pushManager: 'PushManager' in window,
  backgroundSync: 'sync' in ServiceWorkerRegistration.prototype,
  periodicSync: 'periodicSync' in ServiceWorkerRegistration.prototype,
  share: 'share' in navigator,
  badging: 'setAppBadge' in navigator,
  fileSystem: 'showOpenFilePicker' in window,
  wakeLock: 'wakeLock' in navigator,
  contacts: 'contacts' in navigator
};

console.log('Fonctionnalités supportées:', features);
```

---

## Performance et Optimisation

### 1. Preloading et Prefetching

```html
<!-- Preload des ressources critiques -->
<link rel="preload" href="/styles/critical.css" as="style">
<link rel="preload" href="/scripts/main.js" as="script">
<link rel="preload" href="/fonts/main.woff2" as="font" type="font/woff2" crossorigin>

<!-- Prefetch pour la navigation future -->
<link rel="prefetch" href="/page2.html">
<link rel="dns-prefetch" href="https://api.example.com">
```

### 2. Lazy Loading

```javascript
// Images
<img src="placeholder.jpg" data-src="real-image.jpg" loading="lazy" alt="...">

// Scripts
if ('IntersectionObserver' in window) {
  const lazyImages = document.querySelectorAll('img[data-src]');
  const imageObserver = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        const img = entry.target;
        img.src = img.dataset.src;
        imageObserver.unobserve(img);
      }
    });
  });
  
  lazyImages.forEach((img) => imageObserver.observe(img));
}
```

### 3. Code Splitting

```javascript
// Import dynamique
const module = await import('./module.js');

// Avec React
const Component = lazy(() => import('./Component'));
```

### 4. Compression des Assets

```javascript
// Service Worker avec compression Brotli
self.addEventListener('fetch', (event) => {
  const url = new URL(event.request.url);
  
  if (url.pathname.endsWith('.js') || url.pathname.endsWith('.css')) {
    event.respondWith(
      caches.match(event.request).then((response) => {
        if (response) return response;
        
        return fetch(event.request.url, {
          headers: { 'Accept-Encoding': 'br, gzip, deflate' }
        });
      })
    );
  }
});
```

### 5. IndexedDB pour le stockage local

```javascript
// Ouvrir/créer une base de données
const openDB = () => {
  return new Promise((resolve, reject) => {
    const request = indexedDB.open('MyDatabase', 1);
    
    request.onerror = () => reject(request.error);
    request.onsuccess = () => resolve(request.result);
    
    request.onupgradeneeded = (event) => {
      const db = event.target.result;
      if (!db.objectStoreNames.contains('items')) {
        db.createObjectStore('items', { keyPath: 'id', autoIncrement: true });
      }
    };
  });
};

// Ajouter des données
const addData = async (data) => {
  const db = await openDB();
  const transaction = db.transaction(['items'], 'readwrite');
  const store = transaction.objectStore('items');
  store.add(data);
  return transaction.complete;
};

// Lire des données
const getData = async (id) => {
  const db = await openDB();
  const transaction = db.transaction(['items'], 'readonly');
  const store = transaction.objectStore('items');
  return store.get(id);
};
```

### 6. App Shell Pattern

```javascript
// Service Worker avec App Shell
const APP_SHELL = [
  '/',
  '/index.html',
  '/styles/main.css',
  '/scripts/app.js',
  '/shell.html'
];

self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open('app-shell-v1')
      .then((cache) => cache.addAll(APP_SHELL))
  );
});
```

---

## Debugging et Outils

### 1. Chrome DevTools

```javascript
// Console du Service Worker
chrome://serviceworker-internals/

// Lighthouse PWA Audit
// DevTools > Lighthouse > Progressive Web App

// Application Panel
// DevTools > Application > Service Workers
// DevTools > Application > Manifest
// DevTools > Application > Storage
```

### 2. Workbox (Bibliothèque Google)

```javascript
// Installation
npm install workbox-webpack-plugin --save-dev

// Configuration webpack
const { GenerateSW } = require('workbox-webpack-plugin');

module.exports = {
  plugins: [
    new GenerateSW({
      clientsClaim: true,
      skipWaiting: true,
      runtimeCaching: [
        {
          urlPattern: /^https:\/\/api\./,
          handler: 'NetworkFirst',
          options: {
            cacheName: 'api-cache',
            expiration: {
              maxEntries: 50,
              maxAgeSeconds: 300
            }
          }
        }
      ]
    })
  ]
};
```

```javascript
// Utilisation directe de Workbox
importScripts('https://storage.googleapis.com/workbox-cdn/releases/6.5.4/workbox-sw.js');

workbox.routing.registerRoute(
  ({request}) => request.destination === 'image',
  new workbox.strategies.CacheFirst({
    cacheName: 'images',
    plugins: [
      new workbox.expiration.ExpirationPlugin({
        maxEntries: 60,
        maxAgeSeconds: 30 * 24 * 60 * 60 // 30 jours
      })
    ]
  })
);
```

### 3. Testing

```javascript
// Test du Service Worker avec Jest
describe('Service Worker', () => {
  it('should cache resources on install', async () => {
    const cache = await caches.open('test-cache');
    const response = await cache.match('/index.html');
    expect(response).toBeDefined();
  });
});
```

### 4. Monitoring

```javascript
// Rapport d'erreur dans le Service Worker
self.addEventListener('error', (event) => {
  console.error('Erreur SW:', event.error);
  // Envoyer à votre service de monitoring
});

// Performance monitoring
const perfObserver = new PerformanceObserver((list) => {
  list.getEntries().forEach((entry) => {
    console.log('Navigation timing:', entry);
  });
});
perfObserver.observe({ entryTypes: ['navigation'] });
```

### 5. Update Strategy

```javascript
// Forcer la mise à jour du Service Worker
async function checkForUpdates() {
  const registration = await navigator.serviceWorker.ready;
  await registration.update();
  
  registration.addEventListener('updatefound', () => {
    const newWorker = registration.installing;
    
    newWorker.addEventListener('statechange', () => {
      if (newWorker.state === 'installed' && navigator.serviceWorker.controller) {
        // Nouvelle version disponible
        showUpdateNotification();
      }
    });
  });
}

function showUpdateNotification() {
  if (confirm('Nouvelle version disponible. Recharger ?')) {
    window.location.reload();
  }
}

// Vérifier les mises à jour régulièrement
setInterval(checkForUpdates, 60 * 60 * 1000); // Toutes les heures
```

---

## Checklist PWA Complète

### Basique
- [ ] HTTPS activé
- [ ] manifest.json créé et lié
- [ ] Service Worker enregistré
- [ ] Icons (192x192 et 512x512)
- [ ] Responsive design
- [ ] Meta tags appropriés

### Avancé
- [ ] Stratégie de cache optimisée
- [ ] Offline fallback page
- [ ] Push notifications configurées
- [ ] Background sync implémenté
- [ ] App Shell pattern
- [ ] Lazy loading des images
- [ ] Code splitting

### Nouveautés 2024-2025
- [ ] Screenshots dans le manifest
- [ ] File handlers configurés
- [ ] Protocol handlers
- [ ] Window Controls Overlay
- [ ] Launch handler
- [ ] Manifest ID défini
- [ ] Badging API
- [ ] Periodic Background Sync
- [ ] File System Access
- [ ] Screen Wake Lock

### Performance
- [ ] Lighthouse score PWA > 90
- [ ] First Contentful Paint < 1.8s
- [ ] Time to Interactive < 3.8s
- [ ] Assets compressés (Brotli/Gzip)
- [ ] Images optimisées (WebP)
- [ ] Fonts préchargées

### Testing
- [ ] Test sur Chrome, Firefox, Safari
- [ ] Test sur iOS et Android
- [ ] Test offline
- [ ] Test d'installation
- [ ] Test des notifications
- [ ] Test de mise à jour

---

## Ressources Utiles

### Documentation
- [MDN PWA Guide](https://developer.mozilla.org/fr/docs/Web/Progressive_web_apps)
- [web.dev PWA](https://web.dev/progressive-web-apps/)
- [W3C Manifest Spec](https://www.w3.org/TR/appmanifest/)

### Outils
- [Lighthouse](https://developers.google.com/web/tools/lighthouse)
- [PWA Builder](https://www.pwabuilder.com/)
- [Workbox](https://developers.google.com/web/tools/workbox)
- [Manifest Generator](https://app-manifest.firebaseapp.com/)

### Testing
- [Chrome DevTools](https://developer.chrome.com/docs/devtools/)
- [BrowserStack](https://www.browserstack.com/)
- [WebPageTest](https://www.webpagetest.org/)



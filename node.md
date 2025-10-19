# Node.js CheatSheet 

## Table des matières

- [Installation & Versions](#installation--versions)
- [Modules & Imports](#modules--imports)
- [File System (fs)](#file-system-fs)
- [Path](#path)
- [HTTP & HTTPS](#http--https)
- [Events](#events)
- [Streams](#streams)
- [Process](#process)
- [Child Process](#child-process)
- [Async/Await & Promises](#asyncawait--promises)
- [Test Runner (Nouveau)](#test-runner-nouveau)
- [Watch Mode (Nouveau)](#watch-mode-nouveau)
- [NPM & Package Management](#npm--package-management)
- [Environment Variables](#environment-variables)
- [Buffers](#buffers)
- [Crypto](#crypto)
- [Os](#os)
- [URL](#url)
- [Query Strings](#query-strings)
- [Timers](#timers)
- [Util](#util)
- [Zlib](#zlib)
- [Performance Hooks](#performance-hooks)
- [Worker Threads](#worker-threads)
- [Best Practices](#best-practices)

---

## Installation & Versions

```bash
# Installer Node.js (via nvm recommandé)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install node          # Dernière version
nvm install --lts         # Version LTS
nvm use 22                # Utiliser Node.js 22

# Vérifier la version
node --version            # v22.x.x
npm --version             # 10.x.x

# Versions LTS actuelles
# Node.js 20 (Iron) - LTS jusqu'en avril 2026
# Node.js 22 (Jod) - LTS à partir d'octobre 2024
```

---

## Modules & Imports

### CommonJS (traditionnel)

```javascript
// Exporter
module.exports = function() { };
module.exports.name = 'value';
exports.name = 'value';  // Raccourci

// Importer
const module = require('./module');
const { name } = require('./module');
const fs = require('fs');
const path = require('node:path');  // Préfixe node: recommandé
```

### ES Modules (moderne)

```javascript
// package.json
{
  "type": "module"  // Active ES modules par défaut
}

// Exporter
export default function() { }
export const name = 'value';
export { name, value };

// Importer
import module from './module.js';  // .js requis
import { name } from './module.js';
import * as all from './module.js';
import fs from 'node:fs';
import { readFile } from 'node:fs/promises';

// Import dynamique
const module = await import('./module.js');

// Top-level await (Node.js 14.8+)
const data = await fetch('https://api.example.com');
```

### Import Assertions (Node.js 17+)

```javascript
// JSON
import data from './data.json' assert { type: 'json' };

// Node.js 20.10+ - Import Attributes (nouvelle syntaxe)
import data from './data.json' with { type: 'json' };
```

---

## File System (fs)

### API Promises (recommandé)

```javascript
import { readFile, writeFile, appendFile, unlink, mkdir, rm, 
         readdir, stat, access, copyFile, rename } from 'node:fs/promises';

// Lire un fichier
const content = await readFile('file.txt', 'utf8');

// Écrire un fichier
await writeFile('file.txt', 'content', 'utf8');

// Ajouter du contenu
await appendFile('file.txt', 'more content');

// Supprimer un fichier
await unlink('file.txt');

// Créer un dossier
await mkdir('dir', { recursive: true });

// Supprimer un dossier (Node.js 14.14+)
await rm('dir', { recursive: true, force: true });

// Lire un dossier
const files = await readdir('dir');

// Lister avec types (Node.js 20+)
const entries = await readdir('dir', { withFileTypes: true });
for (const entry of entries) {
  console.log(entry.name, entry.isDirectory());
}

// Informations sur un fichier
const stats = await stat('file.txt');
console.log(stats.size, stats.isFile(), stats.isDirectory());

// Vérifier l'existence
try {
  await access('file.txt');
  console.log('Le fichier existe');
} catch {
  console.log('Le fichier n\'existe pas');
}

// Copier un fichier
await copyFile('source.txt', 'dest.txt');

// Renommer/déplacer
await rename('old.txt', 'new.txt');

// Watch (Node.js 19.1+)
import { watch } from 'node:fs/promises';
const watcher = watch('file.txt');
for await (const event of watcher) {
  console.log(event.eventType, event.filename);
}
```

### API Synchrone

```javascript
import { readFileSync, writeFileSync, existsSync, mkdirSync } from 'node:fs';

const content = readFileSync('file.txt', 'utf8');
writeFileSync('file.txt', 'content');
const exists = existsSync('file.txt');
mkdirSync('dir', { recursive: true });
```

### API Callback (legacy)

```javascript
import { readFile, writeFile } from 'node:fs';

readFile('file.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

### Streams (pour gros fichiers)

```javascript
import { createReadStream, createWriteStream } from 'node:fs';

// Lire
const readStream = createReadStream('large.txt', 'utf8');
readStream.on('data', chunk => console.log(chunk));
readStream.on('end', () => console.log('Terminé'));

// Écrire
const writeStream = createWriteStream('output.txt');
writeStream.write('data\n');
writeStream.end();

// Pipeline (recommandé)
import { pipeline } from 'node:stream/promises';
await pipeline(
  createReadStream('input.txt'),
  createWriteStream('output.txt')
);
```

---

## Path

```javascript
import path from 'node:path';

// Joindre des chemins
path.join('/foo', 'bar', 'baz.txt');           // /foo/bar/baz.txt
path.resolve('foo', 'bar', 'baz.txt');         // /current/dir/foo/bar/baz.txt

// Informations
path.basename('/foo/bar/baz.txt');             // baz.txt
path.basename('/foo/bar/baz.txt', '.txt');     // baz
path.dirname('/foo/bar/baz.txt');              // /foo/bar
path.extname('/foo/bar/baz.txt');              // .txt

// Parser un chemin
path.parse('/foo/bar/baz.txt');
// { root: '/', dir: '/foo/bar', base: 'baz.txt', ext: '.txt', name: 'baz' }

// Formater un chemin
path.format({
  root: '/',
  dir: '/foo/bar',
  base: 'baz.txt'
});  // /foo/bar/baz.txt

// Chemin relatif
path.relative('/foo/bar', '/foo/baz/file.txt');  // ../baz/file.txt

// Normaliser
path.normalize('/foo/bar//baz/../file.txt');     // /foo/bar/file.txt

// Path séparateur
path.sep;      // '/' sur Unix, '\\' sur Windows
path.delimiter; // ':' sur Unix, ';' sur Windows

// Chemin absolu?
path.isAbsolute('/foo/bar');  // true
path.isAbsolute('bar');       // false

// Constantes utiles
import { fileURLToPath } from 'node:url';
const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);
```

---

## HTTP & HTTPS

### Serveur HTTP basique

```javascript
import http from 'node:http';

const server = http.createServer((req, res) => {
  // Headers
  res.setHeader('Content-Type', 'text/html');
  res.writeHead(200, { 'Content-Type': 'application/json' });
  
  // Réponse
  res.write('Hello World');
  res.end(JSON.stringify({ message: 'ok' }));
});

server.listen(3000, () => {
  console.log('Serveur sur port 3000');
});

// Fermer proprement
process.on('SIGTERM', () => {
  server.close(() => {
    console.log('Serveur fermé');
  });
});
```

### Routing basique

```javascript
const server = http.createServer((req, res) => {
  const url = new URL(req.url, `http://${req.headers.host}`);
  
  if (url.pathname === '/' && req.method === 'GET') {
    res.end('Home');
  } else if (url.pathname === '/api' && req.method === 'POST') {
    let body = '';
    req.on('data', chunk => body += chunk);
    req.on('end', () => {
      const data = JSON.parse(body);
      res.end(JSON.stringify({ received: data }));
    });
  } else {
    res.writeHead(404);
    res.end('Not Found');
  }
});
```

### Requête HTTP (client)

```javascript
import http from 'node:http';

// Méthode callback
http.get('http://api.example.com/data', (res) => {
  let data = '';
  res.on('data', chunk => data += chunk);
  res.on('end', () => console.log(data));
});

// POST
const postData = JSON.stringify({ key: 'value' });
const options = {
  hostname: 'api.example.com',
  port: 80,
  path: '/data',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Content-Length': Buffer.byteLength(postData)
  }
};

const req = http.request(options, (res) => {
  res.on('data', chunk => console.log(chunk.toString()));
});
req.write(postData);
req.end();
```

### Fetch API (Node.js 18+) ⭐ NOUVEAU

```javascript
// Fetch natif (plus besoin de node-fetch)
const response = await fetch('https://api.example.com/data');
const data = await response.json();

// POST
const response = await fetch('https://api.example.com/data', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ key: 'value' })
});

// Avec timeout
const response = await fetch('https://api.example.com/data', {
  signal: AbortSignal.timeout(5000)  // 5 secondes
});

// Streaming response
const response = await fetch('https://example.com/large-file');
for await (const chunk of response.body) {
  console.log(chunk);
}
```

### HTTPS

```javascript
import https from 'node:https';
import fs from 'node:fs';

// Serveur HTTPS
const options = {
  key: fs.readFileSync('key.pem'),
  cert: fs.readFileSync('cert.pem')
};

https.createServer(options, (req, res) => {
  res.end('Secure!');
}).listen(443);

// Requête HTTPS
https.get('https://api.example.com', (res) => {
  // ...
});
```

---

## Events

```javascript
import { EventEmitter } from 'node:events';

class MyEmitter extends EventEmitter {}
const emitter = new MyEmitter();

// Écouter un événement
emitter.on('event', (arg1, arg2) => {
  console.log('Event triggered', arg1, arg2);
});

// Écouter une seule fois
emitter.once('event', () => {
  console.log('Triggered once');
});

// Émettre un événement
emitter.emit('event', 'arg1', 'arg2');

// Retirer un listener
const listener = () => console.log('Hello');
emitter.on('event', listener);
emitter.off('event', listener);  // ou removeListener

// Retirer tous les listeners
emitter.removeAllListeners('event');

// Nombre de listeners
emitter.listenerCount('event');

// Liste des événements
emitter.eventNames();

// Événement d'erreur (toujours écouter!)
emitter.on('error', (err) => {
  console.error('Error:', err);
});

// Events async avec promises (Node.js 11.13+)
import { once, on } from 'node:events';

// Attendre un événement
const [value] = await once(emitter, 'event');

// Itérer sur les événements
for await (const [value] of on(emitter, 'event')) {
  console.log(value);
  if (condition) break;
}
```

---

## Streams

### Types de Streams

```javascript
import { Readable, Writable, Transform, Duplex, pipeline } from 'node:stream';
import { pipeline as pipelinePromise } from 'node:stream/promises';

// Readable Stream
const readable = new Readable({
  read() {
    this.push('data');
    this.push(null);  // Fin du stream
  }
});

// Writable Stream
const writable = new Writable({
  write(chunk, encoding, callback) {
    console.log(chunk.toString());
    callback();
  }
});

// Transform Stream
const transform = new Transform({
  transform(chunk, encoding, callback) {
    this.push(chunk.toString().toUpperCase());
    callback();
  }
});

// Duplex Stream (lecture et écriture)
const duplex = new Duplex({
  read() { this.push('data'); this.push(null); },
  write(chunk, encoding, callback) { callback(); }
});
```

### Pipeline (recommandé)

```javascript
import { createReadStream, createWriteStream } from 'node:fs';
import { createGzip } from 'node:zlib';
import { pipeline } from 'node:stream/promises';

// Avec promises
await pipeline(
  createReadStream('input.txt'),
  createGzip(),
  createWriteStream('input.txt.gz')
);

// Callback style
import { pipeline as pipelineCb } from 'node:stream';
pipelineCb(
  createReadStream('input.txt'),
  createWriteStream('output.txt'),
  (err) => {
    if (err) console.error('Pipeline failed', err);
    else console.log('Pipeline succeeded');
  }
);
```

### Async Iterators (Node.js 10+)

```javascript
import { createReadStream } from 'node:fs';

const stream = createReadStream('file.txt', 'utf8');

for await (const chunk of stream) {
  console.log(chunk);
}
```

### Stream Utilities

```javascript
import { Readable } from 'node:stream';

// Créer un readable depuis un array (Node.js 12.3+)
const stream = Readable.from(['data1', 'data2', 'data3']);

// Créer depuis un async generator
async function* generate() {
  yield 'chunk1';
  yield 'chunk2';
}
const stream2 = Readable.from(generate());

// Collecter tout le stream (Node.js 16.14+)
import { text, buffer, json } from 'node:stream/consumers';
const content = await text(stream);
const buf = await buffer(stream);
const data = await json(stream);

// Stream.compose (Node.js 16.9+)
import { compose } from 'node:stream';
const composed = compose(transform1, transform2, transform3);
```

---

## Process

```javascript
// Informations sur le processus
process.pid;              // ID du processus
process.version;          // Version de Node.js
process.versions;         // Versions des dépendances
process.arch;             // Architecture (x64, arm, etc.)
process.platform;         // Plateforme (linux, darwin, win32)
process.uptime();         // Temps d'exécution en secondes

// Arguments
process.argv;             // ['node', 'script.js', 'arg1', 'arg2']
process.argv.slice(2);    // ['arg1', 'arg2']

// Répertoires
process.cwd();            // Répertoire courant
process.chdir('/path');   // Changer de répertoire

// Environnement
process.env.NODE_ENV;     // Variables d'environnement
process.env.PATH;

// Sortie
process.exit(0);          // Quitter (0 = succès, 1+ = erreur)
process.exitCode = 1;     // Définir le code de sortie

// Signaux
process.on('SIGTERM', () => {
  console.log('Termination signal received');
  process.exit(0);
});

process.on('SIGINT', () => {  // Ctrl+C
  console.log('Interrupted');
  process.exit(0);
});

// Événements
process.on('exit', (code) => {
  console.log(`Exiting with code ${code}`);
});

process.on('uncaughtException', (err) => {
  console.error('Uncaught exception:', err);
  process.exit(1);
});

process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled rejection:', reason);
});

// Warning
process.on('warning', (warning) => {
  console.warn(warning.name, warning.message);
});

// Mémoire
process.memoryUsage();
// { rss, heapTotal, heapUsed, external, arrayBuffers }

// CPU
process.cpuUsage();

// Standard streams
process.stdin;            // Readable stream
process.stdout;           // Writable stream
process.stderr;           // Writable stream

// Lecture stdin
process.stdin.on('data', (data) => {
  console.log(`Received: ${data}`);
});

// Next tick
process.nextTick(() => {
  console.log('Executed before I/O');
});
```

---

## Child Process

```javascript
import { exec, execSync, spawn, fork } from 'node:child_process';
import { promisify } from 'node:util';

// exec - commande simple
exec('ls -la', (error, stdout, stderr) => {
  if (error) console.error(error);
  console.log(stdout);
});

// exec avec promises
const execPromise = promisify(exec);
const { stdout, stderr } = await execPromise('ls -la');

// execSync - synchrone (bloquant)
const output = execSync('ls -la', { encoding: 'utf8' });

// spawn - processus avec streams (recommandé pour gros output)
const child = spawn('find', ['.', '-type', 'f']);

child.stdout.on('data', (data) => {
  console.log(`stdout: ${data}`);
});

child.stderr.on('data', (data) => {
  console.error(`stderr: ${data}`);
});

child.on('close', (code) => {
  console.log(`Exited with code ${code}`);
});

// fork - processus Node.js séparé
const forked = fork('child.js');

// Communication IPC
forked.send({ hello: 'world' });
forked.on('message', (msg) => {
  console.log('Message from child:', msg);
});

// Dans child.js
process.on('message', (msg) => {
  console.log('Message from parent:', msg);
  process.send({ reply: 'ok' });
});

// Tuer un processus
child.kill('SIGTERM');
```

---

## Async/Await & Promises

```javascript
// Promise basique
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve('Success'), 1000);
});

promise
  .then(result => console.log(result))
  .catch(error => console.error(error))
  .finally(() => console.log('Done'));

// Async/Await (recommandé)
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error:', error);
    throw error;
  }
}

// Promise.all - parallèle
const [result1, result2, result3] = await Promise.all([
  fetch('url1'),
  fetch('url2'),
  fetch('url3')
]);

// Promise.allSettled - attend toutes (Node.js 12.9+)
const results = await Promise.allSettled([
  fetch('url1'),
  fetch('url2')
]);
// [{ status: 'fulfilled', value: ... }, { status: 'rejected', reason: ... }]

// Promise.race - première qui termine
const result = await Promise.race([
  fetch('url1'),
  fetch('url2')
]);

// Promise.any - première qui réussit (Node.js 15+)
const result = await Promise.any([
  fetch('url1'),
  fetch('url2')
]);

// Promisify callback functions
import { promisify } from 'node:util';
import { readFile } from 'node:fs';

const readFilePromise = promisify(readFile);
const content = await readFilePromise('file.txt', 'utf8');

// Top-level await (ES modules)
const data = await fetch('https://api.example.com');

// Async iterators
async function* asyncGenerator() {
  yield await Promise.resolve(1);
  yield await Promise.resolve(2);
}

for await (const num of asyncGenerator()) {
  console.log(num);
}
```

---

## Test Runner (Nouveau) ⭐

Node.js 18+ inclut un test runner natif!

```javascript
// test/example.test.js
import { test, describe, it, before, after, beforeEach, afterEach } from 'node:test';
import assert from 'node:assert';

// Test simple
test('addition works', () => {
  assert.strictEqual(2 + 2, 4);
});

// Test async
test('async test', async () => {
  const result = await Promise.resolve(42);
  assert.strictEqual(result, 42);
});

// Describe/it style
describe('Calculator', () => {
  it('should add numbers', () => {
    assert.strictEqual(1 + 1, 2);
  });

  it('should subtract numbers', () => {
    assert.strictEqual(5 - 3, 2);
  });
});

// Hooks
describe('Database tests', () => {
  before(async () => {
    // Setup avant tous les tests
    await connectDatabase();
  });

  after(async () => {
    // Cleanup après tous les tests
    await disconnectDatabase();
  });

  beforeEach(() => {
    // Avant chaque test
  });

  afterEach(() => {
    // Après chaque test
  });

  it('should insert data', async () => {
    // Test
  });
});

// Subtests
test('parent test', async (t) => {
  await t.test('subtest 1', () => {
    assert.ok(true);
  });

  await t.test('subtest 2', () => {
    assert.ok(true);
  });
});

// Skip tests
test('this test is skipped', { skip: true }, () => {
  // Won't run
});

test('conditionally skip', { skip: process.platform === 'win32' }, () => {
  // Skipped on Windows
});

// Only run specific tests
test('only this test', { only: true }, () => {
  // Only this will run
});

// Timeout
test('test with timeout', { timeout: 5000 }, async () => {
  await slowOperation();
});

// Mocking (Node.js 19+)
import { mock } from 'node:test';

test('mocking functions', (t) => {
  const mockFn = t.mock.fn((a, b) => a + b);
  
  mockFn(1, 2);
  mockFn(3, 4);
  
  assert.strictEqual(mockFn.mock.calls.length, 2);
  assert.deepStrictEqual(mockFn.mock.calls[0].arguments, [1, 2]);
  assert.strictEqual(mockFn.mock.calls[0].result, 3);
});

// Mock methods
test('mocking object methods', (t) => {
  const obj = {
    method: () => 'original'
  };
  
  t.mock.method(obj, 'method', () => 'mocked');
  assert.strictEqual(obj.method(), 'mocked');
});

// Snapshots (Node.js 22+)
test('snapshot test', (t) => {
  const data = { name: 'John', age: 30 };
  t.assert.snapshot(data);
});
```

### Exécuter les tests

```bash
# Exécuter tous les tests
node --test

# Tests spécifiques
node --test test/example.test.js

# Avec watch mode (Node.js 19+)
node --test --watch

# Avec coverage (Node.js 20.1+)
node --test --experimental-test-coverage

# Pattern matching
node --test --test-name-pattern="should add"

# Parallel execution
node --test --test-concurrency=4
```

---

## Watch Mode (Nouveau) ⭐

Node.js 18.11+ supporte le watch mode natif!

```bash
# Watch mode pour exécution
node --watch script.js

# Watch mode pour tests
node --test --watch

# Watch fichiers spécifiques
node --watch-path=./src --watch-path=./lib script.js

# Ignorer certains fichiers
node --watch --watch-preserve-output script.js
```

### API Watch (programmatique)

```javascript
import { watch } from 'node:fs/promises';

const watcher = watch('./src', { recursive: true });

for await (const event of watcher) {
  console.log(`${event.eventType}: ${event.filename}`);
}
```

---

## NPM & Package Management

```bash
# Initialiser un projet
npm init                    # Interactif
npm init -y                # Avec defaults

# Installer des packages
npm install package         # Production dependency
npm install -D package      # Dev dependency
npm install -g package      # Global
npm install package@1.2.3   # Version spécifique
npm install package@latest  # Dernière version

# Mettre à jour
npm update                  # Tous les packages
npm update package          # Package spécifique
npm outdated               # Voir les packages obsolètes

# Désinstaller
npm uninstall package
npm uninstall -g package

# Scripts
npm run script-name
npm start                  # npm run start
npm test                   # npm run test
npm run build

# Information
npm list                   # Packages installés
npm list -g --depth=0     # Packages globaux
npm view package          # Info sur un package
npm search package        # Chercher un package

# Cache
npm cache clean --force

# Audit de sécurité
npm audit                  # Voir les vulnérabilités
npm audit fix             # Corriger automatiquement
npm audit fix --force     # Correction aggressive

# Workspaces (monorepos)
npm install -w package-a   # Installer dans un workspace
npm run test --workspaces  # Exécuter dans tous les workspaces
```

### package.json

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "type": "module",
  "description": "My application",
  "main": "index.js",
  "exports": {
    ".": "./index.js",
    "./utils": "./src/utils.js"
  },
  "scripts": {
    "start": "node index.js",
    "dev": "node --watch index.js",
    "test": "node --test",
    "test:watch": "node --test --watch"
  },
  "dependencies": {
    "express": "^4.18.0"
  },
  "devDependencies": {
    "nodemon": "^3.0.0"
  },
  "engines": {
    "node": ">=18.0.0"
  },
  "keywords": ["nodejs", "api"],
  "author": "Your Name",
  "license": "MIT"
}
```

---

## Environment Variables

```javascript
// Accéder aux variables d'environnement
process.env.NODE_ENV;
process.env.PORT || 3000;
process.env.DATABASE_URL;

// Définir (temporaire)
process.env.MY_VAR = 'value';

// Vérifier si définie
if (process.env.NODE_ENV === 'production') {
  // Production code
}

// .env file (avec dotenv package)
import 'dotenv/config';  // ou require('dotenv').config()

// .env
// NODE_ENV=development
// PORT=3000
// DATABASE_URL=postgresql://localhost/mydb
// API_KEY=secret_key

// Bonne pratique: valeur par défaut
const port = process.env.PORT ?? 3000;
const nodeEnv = process.env.NODE_ENV ?? 'development';
```

---

## Buffers

```javascript
// Créer un buffer
const buf1 = Buffer.from('Hello');
const buf2 = Buffer.from([0x48, 0x65, 0x6c, 0x6c, 0x6f]);
const buf3 = Buffer.alloc(10);        // 10 bytes remplis de zéros
const buf4 = Buffer.allocUnsafe(10);  // Plus rapide mais non initialisé

// Encodages
const buf = Buffer.from('Hello', 'utf8');
const hex = buf.toString('hex');      // '48656c6c6f'
const base64 = buf.toString('base64'); // 'SGVsbG8='
Buffer.from('48656c6c6f', 'hex').toString(); // 'Hello'

// Opérations
buf.length;                    // Taille en bytes
buf[0];                        // Accéder à un byte
buf[0] = 0x48;                // Modifier un byte
buf.write('Hi', 0);           // Écrire à une position

// Copier
const buf5 = Buffer.alloc(5);
buf.copy(buf5, 0, 0, 5);

// Concaténer
const combined = Buffer.concat([buf1, buf2]);

// Comparer
buf1.equals(buf2);            // Égalité
Buffer.compare(buf1, buf2);   // -1, 0, ou 1

// Slice
const slice = buf.slice(0, 3);

// JSON
buf.toJSON();                 // { type: 'Buffer', data: [72, 101, 108, 108, 111] }

// Vérifier si c'est un buffer
Buffer.isBuffer(buf);
```

---

## Crypto

```javascript
import crypto from 'node:crypto';

// Hashing
const hash = crypto.createHash('sha256');
hash.update('data to hash');
const digest = hash.digest('hex');

// Shorthand
const hash2 = crypto.createHash('sha256').update('data').digest('hex');

// HMAC
const hmac = crypto.createHmac('sha256', 'secret-key');
hmac.update('data');
const signature = hmac.digest('hex');

// Random bytes
const bytes = crypto.randomBytes(16);
const hex = bytes.toString('hex');

// Random UUID (Node.js 14.17+)
const uuid = crypto.randomUUID();

// Encryption/Decryption
const algorithm = 'aes-256-cbc';
const key = crypto.randomBytes(32);
const iv = crypto.randomBytes(16);

// Encrypt
const cipher = crypto.createCipheriv(algorithm, key, iv);
let encrypted = cipher.update('secret data', 'utf8', 'hex');
encrypted += cipher.final('hex');

// Decrypt
const decipher = crypto.createDecipheriv(algorithm, key, iv);
let decrypted = decipher.update(encrypted, 'hex', 'utf8');
decrypted += decipher.final('utf8');

// Password hashing (scrypt)
crypto.scrypt('password', 'salt', 64, (err, derivedKey) => {
  if (err) throw err;
  console.log(derivedKey.toString('hex'));
});

// Async version
const derivedKey = await new Promise((resolve, reject) => {
  crypto.scrypt('password', 'salt', 64, (err, key) => {
    if (err) reject(err);
    else resolve(key);
  });
});

// Web Crypto API (Node.js 15+)
const { webcrypto } = crypto;
const key = await webcrypto.subtle.generateKey(
  {
    name: 'AES-GCM',
    length: 256
  },
  true,
  ['encrypt', 'decrypt']
);
```

---

## OS

```javascript
import os from 'node:os';

// Informations système
os.platform();        // 'linux', 'darwin', 'win32'
os.arch();           // 'x64', 'arm', 'arm64'
os.type();           // 'Linux', 'Darwin', 'Windows_NT'
os.release();        // Version du kernel
os.hostname();       // Nom de l'hôte

// CPU
os.cpus();           // Info sur chaque CPU
os.cpus().length;    // Nombre de CPUs

// Mémoire
os.totalmem();       // Mémoire totale en bytes
os.freemem();        // Mémoire libre en bytes

// Utilisateur
os.userInfo();       // { username, uid, gid, shell, homedir }
os.homedir();        // Répertoire home
os.tmpdir();         // Répertoire temporaire

// Uptime
os.uptime();         // Temps de fonctionnement en secondes

// Network
os.networkInterfaces(); // Interfaces réseau

// Constantes
os.EOL;              // '\n' sur Unix, '\r\n' sur Windows
os.devNull;          // '/dev/null' ou '\\\\.\\nul'

// Load average (Unix seulement)
os.loadavg();        // [1min, 5min, 15min]

// Endianness
os.endianness();     // 'BE' ou 'LE'
```

---

## URL

```javascript
import { URL, URLSearchParams, fileURLToPath, pathToFileURL } from 'node:url';

// Parser une URL
const url = new URL('https://user:pass@example.com:8080/path?query=value#hash');

url.protocol;        // 'https:'
url.username;        // 'user'
url.password;        // 'pass'
url.hostname;        // 'example.com'
url.host;           // 'example.com:8080'
url.port;           // '8080'
url.pathname;       // '/path'
url.search;         // '?query=value'
url.searchParams;   // URLSearchParams object
url.hash;           // '#hash'
url.href;           // URL complète
url.origin;         // 'https://example.com:8080'

// Query parameters
const params = url.searchParams;
params.get('query');              // 'value'
params.has('query');              // true
params.set('newkey', 'newvalue');
params.append('key', 'value2');
params.delete('key');
params.toString();                // 'query=value&newkey=newvalue'

// Itérer sur les params
for (const [key, value] of params) {
  console.log(key, value);
}

// URLSearchParams depuis string
const params2 = new URLSearchParams('foo=bar&foo=baz&key=value');
params2.getAll('foo');            // ['bar', 'baz']

// File URLs
const filePath = fileURLToPath('file:///path/to/file.txt');
const fileUrl = pathToFileURL('/path/to/file.txt');

// URL relative
const base = new URL('https://example.com/base/');
const relative = new URL('page.html', base);  // https://example.com/base/page.html
```

---

## Query Strings

```javascript
import querystring from 'node:querystring';

// Parse
const parsed = querystring.parse('foo=bar&abc=xyz&abc=123');
// { foo: 'bar', abc: ['xyz', '123'] }

// Stringify
const str = querystring.stringify({ foo: 'bar', baz: ['qux', 'quux'] });
// 'foo=bar&baz=qux&baz=quux'

// Escape/Unescape
querystring.escape('hello world');    // 'hello%20world'
querystring.unescape('hello%20world'); // 'hello world'

// Note: URLSearchParams est préféré pour les nouvelles apps
const params = new URLSearchParams({ foo: 'bar', baz: 'qux' });
params.toString();  // 'foo=bar&baz=qux'
```

---

## Timers

```javascript
// setTimeout
const timeout = setTimeout(() => {
  console.log('Executed after 1 second');
}, 1000);

clearTimeout(timeout);  // Annuler

// setInterval
const interval = setInterval(() => {
  console.log('Executed every second');
}, 1000);

clearInterval(interval);  // Annuler

// setImmediate (après I/O)
setImmediate(() => {
  console.log('Executed after I/O');
});

// Promises-based timers (Node.js 15+)
import { setTimeout as setTimeoutPromise, setInterval as setIntervalPromise, setImmediate as setImmediatePromise } from 'node:timers/promises';

// Await timeout
await setTimeoutPromise(1000);
console.log('1 second passed');

// Async interval
for await (const tick of setIntervalPromise(1000)) {
  console.log('Tick', tick);
  if (tick === 5) break;
}

// Immediate
await setImmediatePromise();

// AbortSignal support
const ac = new AbortController();
setTimeout(() => ac.abort(), 2000);

try {
  await setTimeoutPromise(5000, undefined, { signal: ac.signal });
} catch (err) {
  if (err.name === 'AbortError') {
    console.log('Timeout aborted');
  }
}
```

---

## Util

```javascript
import util from 'node:util';

// Promisify (callback -> promise)
import { readFile } from 'node:fs';
const readFilePromise = util.promisify(readFile);
const content = await readFilePromise('file.txt', 'utf8');

// Callbackify (promise -> callback) - rarement utilisé
const callbackFn = util.callbackify(async () => 'result');
callbackFn((err, result) => console.log(result));

// Format
util.format('Hello %s', 'world');           // 'Hello world'
util.format('Number: %d', 42);              // 'Number: 42'
util.format('JSON: %j', { key: 'value' });  // 'JSON: {"key":"value"}'

// Inspect (debug)
const obj = { name: 'John', nested: { deep: true } };
util.inspect(obj, { depth: null, colors: true });

// Types checking
util.types.isAsyncFunction(async () => {});  // true
util.types.isPromise(Promise.resolve());     // true
util.types.isDate(new Date());              // true
util.types.isRegExp(/regex/);               // true
util.types.isMap(new Map());                // true
util.types.isSet(new Set());                // true

// Deprecate
const deprecated = util.deprecate(
  () => console.log('Old function'),
  'This function is deprecated'
);

// TextEncoder/TextDecoder
const encoder = new util.TextEncoder();
const uint8array = encoder.encode('Hello');

const decoder = new util.TextDecoder();
const text = decoder.decode(uint8array);

// Parse args (Node.js 18.3+)
import { parseArgs } from 'node:util';
const { values, positionals } = parseArgs({
  args: ['--name', 'John', '--age', '30', 'extra'],
  options: {
    name: { type: 'string' },
    age: { type: 'string' },
    verbose: { type: 'boolean', short: 'v' }
  },
  allowPositionals: true
});
// values: { name: 'John', age: '30' }
// positionals: ['extra']

// MIMEType (Node.js 19.6+)
const mime = new util.MIMEType('text/html; charset=utf-8');
mime.type;      // 'text'
mime.subtype;   // 'html'
mime.essence;   // 'text/html'
```

---

## Zlib

```javascript
import { gzip, gunzip, deflate, inflate, brotliCompress, brotliDecompress } from 'node:zlib';
import { promisify } from 'node:util';

const gzipPromise = promisify(gzip);
const gunzipPromise = promisify(gunzip);

// Compress
const input = Buffer.from('Hello World!');
const compressed = await gzipPromise(input);
console.log('Compressed size:', compressed.length);

// Decompress
const decompressed = await gunzipPromise(compressed);
console.log(decompressed.toString()); // 'Hello World!'

// Stream compression
import { createReadStream, createWriteStream } from 'node:fs';
import { createGzip } from 'node:zlib';
import { pipeline } from 'node:stream/promises';

await pipeline(
  createReadStream('input.txt'),
  createGzip(),
  createWriteStream('input.txt.gz')
);

// Brotli (meilleure compression, Node.js 11.7+)
const brotliCompressPromise = promisify(brotliCompress);
const brotliDecompressPromise = promisify(brotliDecompress);

const brotliCompressed = await brotliCompressPromise(input);
const brotliDecompressed = await brotliDecompressPromise(brotliCompressed);

// Avec options
import { constants } from 'node:zlib';
const compressed2 = await gzipPromise(input, {
  level: constants.Z_BEST_COMPRESSION
});
```

---

## Performance Hooks

```javascript
import { performance, PerformanceObserver } from 'node:perf_hooks';

// Mesurer le temps
const start = performance.now();
// ... code à mesurer
const end = performance.now();
console.log(`Took ${end - start}ms`);

// Marks et measures
performance.mark('start');
// ... code
performance.mark('end');
performance.measure('My Operation', 'start', 'end');

// Observer les performances
const obs = new PerformanceObserver((items) => {
  items.getEntries().forEach((entry) => {
    console.log(`${entry.name}: ${entry.duration}ms`);
  });
});
obs.observe({ entryTypes: ['measure'] });

// Timedify (mesurer une fonction)
import { performance } from 'node:perf_hooks';

function slowFunction() {
  // Code lent
}

const wrapped = performance.timerify(slowFunction);
wrapped();

// Node.js timing
performance.nodeTiming.nodeStart;
performance.nodeTiming.v8Start;
performance.nodeTiming.bootstrapComplete;

// Event Loop Utilization (Node.js 14.10+)
const elu = performance.eventLoopUtilization();
// { idle, active, utilization }
```

---

## Worker Threads

```javascript
// main.js
import { Worker } from 'node:worker_threads';

const worker = new Worker('./worker.js', {
  workerData: { num: 5 }
});

worker.on('message', (result) => {
  console.log('Result from worker:', result);
});

worker.on('error', (err) => {
  console.error('Worker error:', err);
});

worker.on('exit', (code) => {
  console.log(`Worker exited with code ${code}`);
});

// Envoyer un message
worker.postMessage({ command: 'start' });

// worker.js
import { parentPort, workerData, isMainThread } from 'node:worker_threads';

if (!isMainThread) {
  // Code du worker
  console.log('Worker data:', workerData);
  
  parentPort.on('message', (msg) => {
    console.log('Message from parent:', msg);
    
    // Calcul intensif
    const result = heavyComputation(workerData.num);
    
    // Envoyer le résultat
    parentPort.postMessage(result);
  });
}

function heavyComputation(n) {
  // Calcul factorielle
  let result = 1;
  for (let i = 2; i <= n; i++) {
    result *= i;
  }
  return result;
}

// Shared Memory (SharedArrayBuffer)
import { Worker } from 'node:worker_threads';

const sharedBuffer = new SharedArrayBuffer(1024);
const sharedArray = new Int32Array(sharedBuffer);

const worker = new Worker('./worker.js', {
  workerData: { sharedBuffer }
});

// Worker peut modifier sharedArray
sharedArray[0] = 42;

// MessageChannel (communication bidirectionnelle)
import { MessageChannel } from 'node:worker_threads';

const { port1, port2 } = new MessageChannel();

port1.on('message', (msg) => {
  console.log('Received:', msg);
});

port2.postMessage('Hello from port2');
```

---

## Best Practices

### Performance

```javascript
// 1. Utiliser les APIs promises/async-await
import { readFile } from 'node:fs/promises';
const data = await readFile('file.txt', 'utf8');

// 2. Éviter les opérations synchrones bloquantes
// ❌ Bad
const data = readFileSync('large-file.txt');

// ✅ Good
const data = await readFile('large-file.txt');

// 3. Utiliser les streams pour les gros fichiers
import { createReadStream } from 'node:fs';
import { pipeline } from 'node:stream/promises';

await pipeline(
  createReadStream('large-file.txt'),
  processStream,
  createWriteStream('output.txt')
);

// 4. Pool de connexions pour les DB
// Réutiliser les connexions au lieu d'en créer de nouvelles

// 5. Clustering pour utiliser tous les CPUs
import cluster from 'node:cluster';
import os from 'node:os';

if (cluster.isPrimary) {
  const numCPUs = os.cpus().length;
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }
} else {
  // Worker process
  startServer();
}
```

### Sécurité

```javascript
// 1. Valider les entrées utilisateur
function validateEmail(email) {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email);
}

// 2. Utiliser helmet pour Express
import helmet from 'helmet';
app.use(helmet());

// 3. Rate limiting
import rateLimit from 'express-rate-limit';
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100
});
app.use(limiter);

// 4. Ne JAMAIS exposer les secrets
// ❌ Bad
const apiKey = 'sk_live_abc123';

// ✅ Good
const apiKey = process.env.API_KEY;

// 5. Échapper les sorties HTML
import createDOMPurify from 'dompurify';
const clean = DOMPurify.sanitize(dirty);

// 6. Utiliser HTTPS en production
import https from 'node:https';
import fs from 'node:fs';

const options = {
  key: fs.readFileSync('key.pem'),
  cert: fs.readFileSync('cert.pem')
};

https.createServer(options, app).listen(443);

// 7. Gérer les erreurs correctement
process.on('uncaughtException', (err) => {
  console.error('Uncaught exception:', err);
  process.exit(1);
});

process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled rejection:', reason);
  process.exit(1);
});
```

### Structure de projet

```
project/
├── src/
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   ├── middleware/
│   ├── utils/
│   └── services/
├── test/
│   ├── unit/
│   └── integration/
├── config/
│   ├── development.js
│   ├── production.js
│   └── test.js
├── public/
├── .env
├── .env.example
├── .gitignore
├── package.json
├── README.md
└── index.js
```

### Error Handling

```javascript
// Custom error class
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = true;
    Error.captureStackTrace(this, this.constructor);
  }
}

// Async error wrapper
const catchAsync = (fn) => {
  return (req, res, next) => {
    fn(req, res, next).catch(next);
  };
};

// Usage
app.get('/api/users/:id', catchAsync(async (req, res, next) => {
  const user = await User.findById(req.params.id);
  if (!user) {
    throw new AppError('User not found', 404);
  }
  res.json(user);
}));

// Global error handler
app.use((err, req, res, next) => {
  err.statusCode = err.statusCode || 500;
  err.status = err.status || 'error';

  if (process.env.NODE_ENV === 'development') {
    res.status(err.statusCode).json({
      status: err.status,
      error: err,
      message: err.message,
      stack: err.stack
    });
  } else {
    res.status(err.statusCode).json({
      status: err.status,
      message: err.message
    });
  }
});
```

### Logging

```javascript
// Winston logger (package populaire)
import winston from 'winston';

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple()
  }));
}

logger.info('Server started on port 3000');
logger.error('Database connection failed', { error: err });
```

---

## Commandes CLI utiles

```bash
# Node REPL
node                          # Entrer en mode interactif

# Exécuter du code inline
node -e "console.log('Hello')"

# Print evaluation
node -p "process.version"

# Check syntax
node --check script.js

# Debug
node --inspect script.js
node --inspect-brk script.js  # Break au début

# Profiling
node --prof script.js
node --prof-process isolate-*.log

# Memory
node --max-old-space-size=4096 script.js  # 4GB heap

# Experimental features
node --experimental-modules script.js
node --experimental-specifier-resolution=node

# TypeScript direct (Node.js 22.6+)
node --experimental-strip-types script.ts

# Environment
NODE_ENV=production node script.js

# Multiple Node versions (nvm)
nvm list                      # Versions installées
nvm install 20                # Installer Node 20
nvm use 20                    # Utiliser Node 20
nvm alias default 20          # Définir par défaut
```

---

## Snippets utiles

### Configuration TypeScript pour Node.js

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "lib": ["ES2022"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "types": ["node"]
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

### Script de démarrage avec nodemon

```json
{
  "scripts": {
    "start": "node dist/index.js",
    "dev": "node --watch src/index.js",
    "dev:ts": "tsx watch src/index.ts",
    "build": "tsc",
    "test": "node --test",
    "test:watch": "node --test --watch",
    "lint": "eslint .",
    "format": "prettier --write ."
  }
}
```

---

## Ressources

- [Documentation officielle](https://nodejs.org/docs/)
- [NPM Registry](https://www.npmjs.com/)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)
- [Blog officiel Node.js](https://nodejs.org/en/blog/)


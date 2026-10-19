# Pacote do projeto "Laboratorio-modular-19-10" para a IA
# 5 arquivos. Cada um começa com ===== ARQUIVO: caminho =====

## Estrutura
├── .github/
│   └── workflows/
│       └── desktop-exe.yml
├── .sk/
│   └── hub.json
├── css/
│   ├── 00-casca.css
│   ├── 02-importar.css
│   ├── 03-dependencias.css
│   ├── 04-analisar.css
│   ├── 05-plano.css
│   ├── 06-scripts.css
│   ├── 07-terminal.css
│   ├── 08-exportar.css
│   ├── 09-integracao.css
│   ├── 10-celular.css
│   └── 11-lab.css
├── electron/
│   ├── main.cjs
│   └── preload.cjs
├── ferramentas/
│   ├── arrumador.html
│   ├── colador.html
│   ├── faxina.html
│   ├── numerador.html
│   └── preparador.html
├── icons/
│   ├── apple-touch-icon.png
│   ├── icon-128.png
│   ├── icon-144.png
│   ├── icon-152.png
│   ├── icon-16.png
│   ├── icon-180.png
│   ├── icon-192.png
│   ├── icon-32.png
│   ├── icon-384.png
│   ├── icon-48.png
│   ├── icon-512.png
│   ├── icon-72.png
│   ├── icon-96.png
│   ├── icon.svg
│   ├── maskable-192.png
│   ├── maskable-512.png
│   └── maskable.svg
├── js/
│   ├── 00-casca.js
│   ├── 02-importar.js
│   ├── 03-dependencias.js
│   ├── 04-analisar.js
│   ├── 05-plano.js
│   ├── 05a-markdown.js
│   ├── 06-scripts.js
│   ├── 06b-scripts-fim.js
│   ├── 07-terminal.js
│   ├── 07a-busca-npm.js
│   ├── 08-exportar.js
│   ├── 09-integracao.js
│   ├── 10-celular.js
│   ├── 11-lab-base.js
│   ├── 12-lab-mapa.js
│   ├── 13-lab-previa.js
│   ├── 14-lab-pesquisa.js
│   ├── 15-lab-comparar.js
│   ├── 15a-tar.js
│   └── 16-lab-ferramentas.js
├── kits/
│   ├── juntar.py
│   ├── kit-replit.cjs
│   ├── organizar.py
│   └── servir.py
├── lib/
│   ├── jszip.min.js
│   └── pako.min.js
├── .gitignore
├── favicon.ico
├── hub.html
├── hub.webmanifest
├── index.html
├── LEIA-ME.md
├── manifest.json
├── package.json
├── PLANO.md
├── RECEITA.md
└── service-worker.js

===== ARQUIVO: .github/workflows/desktop-exe.yml =====
name: Desktop (.exe)
# Feita pelo 🏗️ Preparar do Mini SK. O .exe sai em Actions → "Desktop (.exe)" → Artifacts.
on:
  workflow_dispatch:

jobs:
  exe:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - name: Instalar as dependências
        run: npm install --no-audit --no-fund --legacy-peer-deps
      - name: Montar o site
        run: npm run build --if-present
      - name: Fazer o .exe
        run: npx electron-builder --win portable --publish never
      - uses: actions/upload-artifact@v4
        with:
          name: app-windows
          path: dist-electron/*.exe
===== FIM: .github/workflows/desktop-exe.yml =====

===== ARQUIVO: electron/main.cjs =====
// Abre o app numa janela de computador (Electron). Gerado pelo 🏗️ Preparar do Mini SK.
// Usa o endereço app://local/ (e não file://) para os módulos, o banco do navegador
// e o service worker funcionarem igual no site.
const { app, BrowserWindow, protocol, net, shell, session } = require('electron');
const path = require('path');
const fs = require('fs');
const { pathToFileURL } = require('url');
const PASTA = path.join(__dirname, '..', ".");

protocol.registerSchemesAsPrivileged([{ scheme: 'app', privileges: { standard: true, secure: true, supportFetchAPI: true, corsEnabled: true, stream: true } }]);

function criarJanela() {
  const w = new BrowserWindow({
    width: 1200, height: 800, autoHideMenuBar: true,
    webPreferences: { preload: path.join(__dirname, 'preload.cjs'), contextIsolation: true, nodeIntegration: false },
  });
  // links de fora abrem no navegador do computador
  w.webContents.setWindowOpenHandler(({ url }) => { if (!url.startsWith('app://')) { shell.openExternal(url); return { action: 'deny' }; } return { action: 'allow' }; });
  w.loadURL('app://local/index.html');
}

app.whenReady().then(() => {
  protocol.handle('app', (req) => {
    let p = decodeURIComponent(new URL(req.url).pathname);
    if (p === '/' || p === '') p = '/index.html';
    let f = path.normalize(path.join(PASTA, p));
    if (!f.startsWith(path.normalize(PASTA))) return new Response('proibido', { status: 403 });
    // app com rotas (/login, /config): volta para o index
    if (!fs.existsSync(f) && !path.extname(f)) f = path.join(PASTA, 'index.html');
    return net.fetch(pathToFileURL(f).toString());
  });
  // microfone, câmera, notificações: libera para o próprio app
  session.defaultSession.setPermissionRequestHandler((wc, perm, cb) => cb(['media', 'notifications', 'clipboard-read', 'clipboard-sanitized-write', 'fullscreen', 'geolocation'].includes(perm)));
  criarJanela();
  app.on('activate', () => { if (BrowserWindow.getAllWindows().length === 0) criarJanela(); });
});
app.on('window-all-closed', () => { if (process.platform !== 'darwin') app.quit(); });
===== FIM: electron/main.cjs =====

===== ARQUIVO: electron/preload.cjs =====
// Ponte segura entre a janela e o computador. A página vê window.desktop.
const { contextBridge } = require('electron');
contextBridge.exposeInMainWorld('desktop', { ehDesktop: true, plataforma: process.platform });
===== FIM: electron/preload.cjs =====

===== ARQUIVO: manifest.json =====
{
  "name": "Laboratorio-modular-19-10",
  "short_name": "Laboratorio-",
  "description": "Laboratorio-modular-19-10",
  "id": "./app-laboratorio-modular-19-10",
  "start_url": "./",
  "scope": "./",
  "display": "standalone",
  "orientation": "any",
  "background_color": "#0f172a",
  "theme_color": "#0f172a",
  "lang": "pt-BR",
  "dir": "ltr",
  "icons": [
    {
      "src": "icons/icon-48.png",
      "sizes": "48x48",
      "type": "image/png",
      "purpose": "any"
    },
    {
      "src": "icons/icon-72.png",
      "sizes": "72x72",
      "type": "image/png",
      "purpose": "any"
    },
    {
      "src": "icons/icon-96.png",
      "sizes": "96x96",
      "type": "image/png",
      "purpose": "any"
    },
    {
      "src": "icons/icon-128.png",
      "sizes": "128x128",
      "type": "image/png",
      "purpose": "any"
    },
    {
      "src": "icons/icon-144.png",
      "sizes": "144x144",
      "type": "image/png",
      "purpose": "any"
    },
    {
      "src": "icons/icon-152.png",
      "sizes": "152x152",
      "type": "image/png",
      "purpose": "any"
    },
    {
      "src": "icons/icon-180.png",
      "sizes": "180x180",
      "type": "image/png",
      "purpose": "any"
    },
    {
      "src": "icons/icon-192.png",
      "sizes": "192x192",
      "type": "image/png",
      "purpose": "any"
    },
    {
      "src": "icons/icon-384.png",
      "sizes": "384x384",
      "type": "image/png",
      "purpose": "any"
    },
    {
      "src": "icons/icon-512.png",
      "sizes": "512x512",
      "type": "image/png",
      "purpose": "any"
    },
    {
      "src": "icons/maskable-192.png",
      "sizes": "192x192",
      "type": "image/png",
      "purpose": "maskable"
    },
    {
      "src": "icons/maskable-512.png",
      "sizes": "512x512",
      "type": "image/png",
      "purpose": "maskable"
    },
    {
      "src": "icons/icon.svg",
      "sizes": "any",
      "type": "image/svg+xml",
      "purpose": "any"
    }
  ]
}
===== FIM: manifest.json =====

===== ARQUIVO: package.json =====
{
  "name": "laboratoriomodular1910",
  "version": "1.0.0",
  "private": true,
  "description": "Laboratorio-modular-19-10",
  "author": "Laboratorio-modular-19-10",
  "main": "electron/main.cjs",
  "scripts": {
    "desktop": "electron .",
    "exe": "electron-builder --win portable --publish never"
  },
  "devDependencies": {
    "electron": "^33.2.0",
    "electron-builder": "^25.1.8"
  },
  "build": {
    "appId": "com.app.laboratoriomodular1910",
    "productName": "Laboratorio-modular-19-10",
    "directories": {
      "output": "dist-electron"
    },
    "files": [
      "**/*",
      "!android/**",
      "!.github/**",
      "!dist-electron/**",
      "!*.zip",
      "!.sk/**"
    ],
    "win": {
      "target": "portable"
    }
  }
}
===== FIM: package.json =====


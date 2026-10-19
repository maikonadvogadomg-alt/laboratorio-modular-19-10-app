# Plano do projeto: Laboratorio-modular-19-10

_Gerado pelo Mini SK em 19/10/2026, 11:02:01_

## Resumo

- Arquivos: **74** (2.0 MB)
- Tipo: projeto Node com 1 package.json
- Arquivos por tipo: JavaScript 26, PNG 15, CSS 11, HTML 7, JSON 3, Python 3, Markdown 3, SVG 2, YAML 1, Git 1, ICO 1, Manifest 1
- Dependências diferentes: **2** (0 obrigatórias, 2 só para montar)
- Restos da Replit: **1 arquivo(s)**

## 📦 laboratoriomodular1910 — `package.json`

- Arquivo principal: `electron/main.cjs`

**Comandos (scripts):**

| Comando | O que faz | Executa |
|---|---|---|
| `desktop` | — | `electron .` |
| `exe` | — | `electron-builder --win portable --publish never` |

**Dependências de montagem (dev) — 2:**

| Pacote | Versão | Pra que serve | Tipo |
|---|---|---|---|
| `electron` | ^33.2.0 | Executável de computador (janela própria) | 💻 EXE |
| `electron-builder` | ^25.1.8 | Gera o .exe do Electron | 💻 EXE |

## ⛔ Restos da Replit (tirar ou trocar)

- ferramentas/preparador.html — 8 menção(ões)

## Arquivos importantes (5) — mandar estes para a IA

- `.github/workflows/desktop-exe.yml`
- `electron/main.cjs`
- `electron/preload.cjs`
- `manifest.json`
- `package.json`

## Estrutura completa (74 arquivos)

```
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
```

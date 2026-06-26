# 🌸 MammaSalute — PWA

App mobile per monitorare **peso e pressione** durante la gravidanza, con sincronizzazione realtime su **Google Sheets**.

## Struttura file

```
mammasalute/
├── index.html          ← App principale
├── manifest.json       ← Configurazione PWA
├── sw.js               ← Service Worker (cache offline)
├── icons/
│   ├── icon-72x72.png
│   ├── icon-96x96.png
│   ├── icon-128x128.png
│   ├── icon-144x144.png
│   ├── icon-152x152.png
│   ├── icon-192x192.png
│   ├── icon-384x384.png
│   └── icon-512x512.png
└── screenshots/
    └── screenshot-portrait.png
```

---

## 1. Configurazione Google OAuth (una volta sola, come sviluppatore)

### A. Crea il progetto su Google Cloud
1. Vai su [console.cloud.google.com](https://console.cloud.google.com)
2. **New Project** → dai un nome (es. "MammaSalute")
3. Seleziona il progetto

### B. Abilita le API
1. **API & Services → Library**
2. Cerca e abilita **Google Sheets API** → Enable
3. Cerca e abilita **Google Drive API** → Enable

### C. Configura la schermata OAuth
1. **API & Services → OAuth consent screen**
2. Tipo: **External** → Create
3. Compila: nome app ("MammaSalute"), email supporto
4. Scopes: aggiungi `spreadsheets` e `drive.file`
5. Test users: aggiungi le email degli utenti finché l'app è in test
   *(Per renderla pubblica a tutti: pubblica l'app → "Publish App")*

### D. Crea il Client ID
1. **API & Services → Credentials → Create Credentials → OAuth 2.0 Client ID**
2. Tipo: **Web application**
3. Nome: "MammaSalute Web"
4. **Authorized JavaScript origins** — aggiungi il dominio dove pubblichi l'app:
   - `https://tuodominio.com`
   - `https://tuousername.github.io` (se usi GitHub Pages)
   - `http://localhost:8080` (per test locale)
5. Copia il **Client ID** generato

### E. Inserisci il Client ID nell'app
Apri `index.html`, trova la riga:
```javascript
const CLIENT_ID = 'IL_TUO_CLIENT_ID.apps.googleusercontent.com';
```
Sostituiscila con il tuo Client ID reale.

---

## 2. Pubblicazione (HTTPS obbligatorio per PWA)

### Opzione A — GitHub Pages (gratuito, consigliato)
```bash
# Crea repository su github.com
git init
git add .
git commit -m "MammaSalute v1"
git remote add origin https://github.com/TUOUSERNAME/mammasalute.git
git push -u origin main

# Abilita GitHub Pages:
# Settings → Pages → Source: main branch → /root
# URL: https://TUOUSERNAME.github.io/mammasalute/
```
Aggiungi `https://TUOUSERNAME.github.io` agli Authorized JavaScript origins su Google Cloud.

### Opzione B — Netlify (drag & drop)
1. Vai su [netlify.com](https://netlify.com)
2. Trascina la cartella `mammasalute/` nella dashboard
3. Ottieni un URL tipo `https://mammasalute.netlify.app`

### Opzione C — Test locale
```bash
cd mammasalute
python3 -m http.server 8080
# Apri http://localhost:8080
```

---

## 3. Installazione su Android

1. Apri l'URL dell'app in **Chrome per Android**
2. Appare il banner "Installa l'app" → tocca per installare
3. Oppure: menu Chrome (⋮) → **"Aggiungi alla schermata Home"**
4. L'app appare come icona nativa, si apre senza barra del browser

---

## Come funziona per gli utenti finali

1. Aprono l'URL dell'app
2. Toccano **"Accedi con Google"** → scelgono il loro account
3. L'app crea automaticamente un file **"MammaSalute – Monitoraggio Gravidanza"** nel loro Google Drive
4. Ogni rilevamento viene salvato in tempo reale nel loro foglio Google Sheets
5. Il file è accessibile anche direttamente da Google Drive / Sheets

---

## Funzionalità

- ✅ Registrazione peso + pressione sistolica/diastolica con data
- ✅ Stepper per settimana di gestazione
- ✅ Note opzionali per ogni rilevamento
- ✅ Grafici andamento peso e pressione
- ✅ Linee di soglia ipertensione (140/90 mmHg)
- ✅ Storico completo con eliminazione
- ✅ Sync realtime Google Sheets
- ✅ PWA installabile su Android
- ✅ Service Worker per funzionamento offline (UI)
- ✅ Ogni utente ha il proprio file Drive separato

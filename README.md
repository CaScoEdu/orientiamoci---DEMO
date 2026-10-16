# 🏫 Mappa Interattiva Scuola (PWA Dual-Event)

Un'applicazione web progressiva (**Progressive Web App - PWA**) modulare ed offline-first per la navigazione indoor in una scuola. Progettata per supportare una gestione **multi-evento** (es. *Open School* e *School Day* ordinario) tramite configurazioni JSON dinamiche, calcolo del percorso minimo mediante algoritmo di Dijkstra su grafo ed isolamento completo delle stanze inattive.

---

## 🚀 Caratteristiche Principali

- 📱 **Dual-PWA & Eventi Indipendenti**: Supporta l'installazione di due WebApp distinte su dispositivi Android (`manifest-open-school.json` e `manifest-school-day.json`) con i propri asset e preferenze.
- 🗺️ **Mappa SVG Vettoriale & Auto-Fit**: Visualizzazione ad alta definizione con zoom adattivo della telecamera (`autoFitCamera`) su sorgente e destinazione.
- 🚶 **Algoritmo di Navigazione (Dijkstra)**: Calcolo dinamico del percorso ottimale tra nodi dell'edificio con tracciamento polilinea animata (`stroke-dasharray`).
- 🚨 **Gestione Emergenza ed Evacuazione**: Visualizzazione immediata delle vie di fuga, punti di raccolta e uscite di sicurezza in base al settore dell'aula.
- 🔗 **Sincronizzazione URL & Web Share API**: Persistenza dello stato della mappa via parametri URL (`?from=...&to=...&config=...`) per la condivisione diretta dei percorsi tramite scheda nativa o appunti.
- 🎨 **Filtro Stanze Disabilitate**: Isolamento visivo ed interattivo per le aule non attive in uno specifico evento (`attivaInEvento: false`).
- 📶 **Offline-First & PWA Toast**: Registrazione del Service Worker per la resilienza offline e notifiche Toast per lo stato della connessione.

---

## 🛠️ Architettura del Progetto

L'applicazione segue un'architettura **Vanilla ES Modules (ESM)** totalmente client-side, senza la necessità di tool di build pesanti (come Webpack o Vite):

```text
.
├── index.html                    # Entrypoint HTML con iniezione dinamica del manifest
├── manifest-open-school.json     # Manifest PWA per l'evento Open School
├── manifest-school-day.json      # Manifest PWA per l'uso scolastico standard
├── sw.js                         # Service Worker per la gestione della cache offline
├── .nojekyll                     # File di configurazione per il deployment su GitHub Pages
│
├── css/
│   └── style.css                 # Reset, variabili cromatiche, animazioni ed adattamento mobile
│
├── js/
│   ├── script.js                 # Controller principale, listener ed eventi PWA
│   ├── ui.js                     # Inizializzazione Tom-Select, gestione modali e dropdowns
│   ├── mapRenderer.js            # Iniezione stili SVG, etichette e disegno percorso
│   ├── config.js                 # Caricamento dinamico configurazioni JSON da URL
│   ├── pathfinder.js             # Algoritmo di Dijkstra per il grafo dei nodi
│   └── camera.js                 # Gestione del ViewBox SVG e calcolo bounds
│
├── config/
│   ├── events/
│   │   ├── open-school.json      # Configurazione aule/stand dell'Open Day
│   │   └── school-day.json       # Configurazione aule per le giornate scolastiche
│   └── navigation/
│       └── grafo.js              # Struttura dati dei nodi e delle connessioni
│
└── assets/
    ├── mappa.svg                 # Planimetria vettoriale dell'Istituto
    └── icons/                    # Icone PWA per i vari eventi (192x192, 512x512)

## 📲 Installazione PWA & QR Code

L'app adatta le sue funzionalità ed il manifest in base al parametro `config` presente nell'URL:

- **Open School (QR Code Visitatori)**:  
  `https://tuo-dominio.github.io/?config=open-school`  
  *Installa l'app "Open School" con lo stand e i laboratori attivi per l'orientamento.*

- **Mappa Scuola (Uso Interno)**:  
  `https://tuo-dominio.github.io/?config=school-day`  
  *Installa l'app "School Day" per la navigazione quotidiana di studenti e personale.*

---

## 📦 Deployment

### GitHub Pages (Consigliato)

1. Effettua il push del codice sulla branch `main` del repository GitHub.
2. Assicurati che il file **`.nojekyll`** sia presente nella radice del progetto.
3. Vai su **Settings** -> **Pages**.
4. Sotto **Build and deployment**, seleziona il branch `main` / `/ (root)` e salva.
5. L'applicazione sarà pubblicata automaticamente con supporto **HTTPS** nativo.

---

## 💻 Sviluppo Locale

Essendo basato su moduli ES nativi (`type="module"`), il progetto richiede di essere eseguito tramite un server HTTP locale:

**Utilizzando VS Code:**
1. Installa l'estensione **Live Server**.
2. Clicca con il tasto destro su `index.html` e seleziona **Open with Live Server**.

**Utilizzando Node.js:**
```bash
npx http-server . -p 8080   

## 📜 Licenza

Questo progetto è distribuito sotto licenza **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)**.  

Sei libero di condividere e adattare questo software per scopi non commerciali, a condizione di attribuire la paternità all'**Istituto Einstein** e distribuire i lavori derivati con la stessa licenza.
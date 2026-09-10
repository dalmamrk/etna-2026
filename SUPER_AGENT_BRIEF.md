# 🌋 ETNA TREKKING 2026 — Dossier Tecnico & Handover per Super Agente AI

> **Destinatario**: Agente AI con accesso in lettura/scrittura alla root di progetto (`ETNA_2026`).  
> **Scopo del documento**: Fornire una visione olistica del codebase, delle scelte architetturali, dei vincoli operativi e della diagnosi tecnica per risolvere il problema segnalato dall'utente:  
> **"Il pulsante 'Mappa Interattiva' non funziona e la mappa non è interattiva"**.

---

## 1. 🧭 Panoramica Generale del Progetto

- **Finalità**: Sito web statico interattivo, *offline-first* e ottimizzato per smartphone (in particolare Safari su iPhone), creato per pianificare e guidare sul campo l'escursione di trekking sull'Etna del **12 Settembre 2026**.
- **Partecipanti**: L'utente e la sua compagna di escursione (il link viene condiviso con lei per navigare la mappa e i percorsi durante il viaggio).
- **Punto di Partenza Comune**: Alloggio a Catania (`Via Crescenzio Galatola 19`).
- **I 3 Itinerari a Confronto**:
  1. **Opzione 1 (Nord)**: Piano Provenzana → Bottoniera Crateri 2002 → Monte Nero.
  2. **Opzione 2 (Sud)**: Rifugio Sapienza → Cratere 2001 → Quota 2.500m (Funivia dell'Etna).
  3. **Opzione 3 (Misto Est/Nord)**: Mattina Sentiero Serracozzo (Grotta e affaccio Valle del Bove) + Pomeriggio Monti Sartorius.
- **Repository GitHub**: [github.com/dalmamrk/etna-2026](https://github.com/dalmamrk/etna-2026)
- **URL Pubblica Live (GitHub Pages)**: [https://dalmamrk.github.io/etna-2026/](https://dalmamrk.github.io/etna-2026/)

---

## 2. 📁 Mappa dei File nella Root

```text
ETNA_2026/
├── etna26.html                  # ⚠️ SORGENTE PRINCIPALE: Tutto il codice HTML, CSS e JS dell'applicazione
├── index.html                   # ⚠️ MIRROR di etna26.html: Usato come entry-point da GitHub Pages (DA SINCRONIZZARE SEMPRE)
├── mobile_preview.html          # Simulatore iPhone (390x844 px) in iframe per test rapido su desktop
├── task.md                      # Changelog e task list completata
├── walkthrough.md               # Documento di passaggio consegne dettagliato
├── SUPER_AGENT_BRIEF.md         # Questo documento di istruzioni per il Super Agente
│
├── [Tracce GPS e KML]
│   ├── serracozzo.gpx           # Traccia reale Serracozzo con oltre 240 quote altimetriche
│   ├── serracozzo_clean.gpx     # Traccia GPX standard ripulita (compatibile Garmin/Komoot)
│   ├── serracozzo_light.gpx     # Versione campionata leggera
│   ├── serracozzo.kml           # Formato KML per Google My Maps ed Earth
│   └── Sentiero per Serra...gpx # File GPX sorgente originale
│
└── images/                      # Tutte le foto locali suddivise per opzione (JPG, WebP, AVIF)
    ├── trekking ai crateri 2002 e monte nero/                           # 7 foto Opzione 1
    ├── Rifugio Sapienza – Cratere del 2001 – Quota 2.500m (Terminale)/ # 7 foto Opzione 2
    └── Sentiero Serracozzo (Valle del Bove)/                             # 11 foto Opzione 3
```

> [!IMPORTANT]
> **Regola Aurea di Sincronizzazione**:  
> Ogni modifica apportata a `etna26.html` deve essere immediatamente replicata in `index.html` (es. `cp etna26.html index.html`), altrimenti il sito pubblicato su GitHub Pages non riceverà gli aggiornamenti!

---

## 3. ⚙️ Architettura Tecnologica e Componenti

1. **Stack Tecnologico**:
   - Zero framework (No React, No Vue, No Tailwind). 100% Vanilla HTML5, CSS moderno e Vanilla JS.
   - **Leaflet.js 1.9.4** caricato via CDN (con CSS relativo) per la cartografia interattiva.
   - **HTML5 Geolocation API**: Modulo GPS Live Tracker con tracciamento continuo `navigator.geolocation.watchPosition`, icona circolare pulsante stile Google Maps, cerchio di precisione e rilevazione altitudine (`coords.altitude`).
2. **Selettori Itinerario (`.controls`)**:
   - Barra bottoni compatta su singola riga per smartphone con switch `.btn-full` (desktop) e `.btn-short` (`TUTTI`, `NORD`, `SUD`, `MISTO`).
3. **Mappa & Switcher Modalità (`.map-mode-bar`)**:
   - Due pulsanti sopra la mappa:
     - `btn-mode-leaflet`: *"Mappa Interattiva (GPS Live)"* → Mostra il container `#map` gestito da Leaflet.
     - `btn-mode-mymaps`: *"Google My Maps (Serracozzo)"* → Mostra l'iframe `#mymaps-view` con la mappa personalizzata Google di Serracozzo (`mid=195sYkebfw2Gy6YnY_fHVYr63Jk5jeug`).
4. **Caroselli Fotografici e Lightbox**:
   - Caroselli dinamici da 7 a 11 foto per scheda con dot indicators e contatore real-time.
   - Modal a tutto schermo con supporto tastiera (frecce, Esc) e navigazione touch.

---

## 4. 🔍 Diagnosi Approfondita del Problema Segnalato

### 🗣️ La Richiesta dell'Utente
> *"il pulsante 'mappa interattiva non funziona' e secondo me la mappa non è interattiva, chiedi di lavorare a questo"*

### 🧪 Analisi Tecnica delle Cause Probabili

Analizzando il codice e il comportamento dell'interfaccia, emergono diverse criticità di UX e di implementazione che portano l'utente a questa conclusione:

#### Causa A: Assenza di Feedback Visivo al Click su "Mappa Interattiva"
- Quando l'utente apre la pagina, la modalità attiva di default è **già** `Mappa Interattiva (GPS Live)`.
- Se l'utente clicca su quel pulsante, `switchMapView('leaflet')` viene eseguita, ma non produce **alcun cambiamento percettibile**: non c'è animazione, non c'è reset dello zoom, non c'è un messaggio toast né una transizione visiva. L'utente percepisce il pulsante come "morto" o "rotto".

#### Causa B: Mancanza di Interattività Esplicita percepita sulla Mappa
L'utente lamenta che *"secondo me la mappa non è interattiva"*. Cosa rende una mappa davvero interattiva per un utente finale?
1. **Tracciati e Sentieri non cliccabili**: Le polilinee dei sentieri (`polyTrekSerracozzo`, `polyTrekNord`, `polyTrekSud`, `roadNord`, ecc.) hanno solo un piccolo `bindTooltip`, ma **non aprono popup dettagliati** al tocco, non evidenziano le statistiche (dislivello, fondo, quota min/max), né consentono di centrare il segmento con un tap.
2. **Tessere OSM e Dipendenza da Rete**: Se la pagina viene testata offline (`file:///`) o in assenza di rete, i tile di OpenStreetMap non vengono scaricati, lasciando uno sfondo grigio con solo le linee vettoriali.
3. **Mancanza di controlli di navigazione avanzati**:
   - Non c'è un pulsante di **Reset Vista / Ricentra Sicilia / Inquadra Etna**.
   - Non c'è uno switcher di layer per passare da vista mappa stradale a **vista satellite** (es. Esri World Imagery) o **mappa topografica/rilievi** (es. OpenTopoMap), che per l'Etna e i crateri vulcanici è spettacolare ed essenziale!
   - Non c'è un pulsante **Schermo Intero (Fullscreen)** per la mappa.
   - Manca un **pannello altimetrico / profilo altimetrico interattivo** (il tracciato di Serracozzo contiene quote reali da 1.738m a 2.303m!).
4. **I pulsanti della barra itinerari (`TUTTI`, `NORD`, `SUD`, `MISTO`) e le Card**:
   - Cliccare sui bottoni filtra le linee sulla mappa, ma se l'utente si trova sul tab Google My Maps, il cambio itinerario non è integrato con My Maps.

---

## 5. 🎯 Obiettivi e Piano di Lavoro per il Super Agente

Il Super Agente deve intervenire su `etna26.html` (e sincronizzare `index.html`) implementando i seguenti miglioramenti:

### Step 1: Ripristinare e Potenziare il Pulsante "Mappa Interattiva"
- Aggiungere feedback visivo immediato (ripple/active state, icona di stato, eventuale breve toast o animazione di rimbalzo/focus).
- Se il pulsante viene cliccato quando la mappa è già attiva, eseguire un'azione utile: **ripristinare l'inquadratura panoramica ottimale (`map.setView([37.65, 15.05], 10)`)** ed eseguire `map.invalidateSize()`.

### Step 2: Rendere la Mappa Veramente e Spettacolarmene Interattiva
1. **Layer Switcher (Topografico / Satellite / Stradale)**:
   - Aggiungere il supporto a un layer **Satellite / Ortofoto** gratuito e affidabile (es. Esri World Imagery: `https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}`) e/o **Rilievo/Topografico** (es. OpenTopoMap o CARTO Dark/Voyager).
   - Permettere all'utente di cambiare layer con un selettore comodo e touch-friendly. Vedere le colate laviche e i crateri dal satellite trasforma radicalmente l'esperienza!
2. **Popup Informativi Ricchi sui Tracciati e sui Marker**:
   - Cliccando su qualsiasi traccia (es. Sentiero Serracozzo verde, Salita Sud ambra, Crateri 2002 blu), la polilinea deve pulsare o evidenziarsi e mostrare un **popup interattivo** con:
     - Nome sentiero, dislivello (+400m), quota massima, tempo stimato, fondo e difficoltà.
     - Pulsante *"Avvia navigazione verso il parcheggio"* o *"Apri traccia gpx"*.
3. **Pulsante Schermo Intero (Fullscreen Toggle)**:
   - Permettere di espandere la mappa a tutto schermo con un pulsante dedicato, comodissimo su iPhone durante il trekking.
4. **Pulsante "Ricentra Mappa" (Reset View)**:
   - Un pulsante con bussola o mirino per tornare all'inquadratura iniziale in qualsiasi momento.
5. **Verifica della Gestione Eventi Touch / Mouse**:
   - Assicurarsi che `dragging`, `touchZoom`, `doubleClickZoom` e `scrollWheelZoom` siano attivi e fluidi su tutti i dispositivi.

### Step 3: Test, Verifica e Sincronizzazione
1. Testare il funzionamento nel browser e su `mobile_preview.html`.
2. Verificare l'assenza di errori nella console (`F12`).
3. Sincronizzare: `cp etna26.html index.html`.
4. Eseguire commit e push su GitHub.
5. Aggiornare `walkthrough.md` e `task.md`.

---

## 6. 📌 Riferimenti Chiave nel Codice

- **Inizializzazione Mappa Leaflet**: `etna26.html` (cerca `const map = L.map('map')`).
- **Traccia Serracozzo ad alta risoluzione**: array `trekSerracozzoFull` (oltre 240 waypoint lat/lng/alt).
- **Logica switcher di modalità**: funzione `switchMapView(mode, scrollToMap)` in fondo allo script.
- **Logica selezione itinerario**: funzione `selectRoute(routeKey)`.
- **Live Tracker GPS**: IIFE anonima in fondo allo script che crea `#gps-toggle`, `#gps-toast` e `#gps-elevation`.

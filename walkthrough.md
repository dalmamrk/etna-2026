# Walkthrough & Handover: Etna Trekking 2026

Questo documento riassume lo stato completo del progetto, le scelte tecniche, l'architettura dei componenti e le modifiche apportate, pensato per il passaggio di consegne a un altro agente AI o sviluppatore.

---

## 🧭 Panoramica del Progetto

- **Repository GitHub**: [github.com/dalmamrk/etna-2026](https://github.com/dalmamrk/etna-2026)
- **URL Pubblica Live (GitHub Pages)**: [https://dalmamrk.github.io/etna-2026/](https://dalmamrk.github.io/etna-2026/)
- **Finalità**: Sito web statico interattivo, self-contained e offline-first per pianificare l'escursione di trekking sull'Etna del 12 Settembre 2026.
- **Punto di Partenza Comune**: Alloggio a Catania (Via Crescenzio Galatola 19).
- **Destinazione**: 3 itinerari alternativi sull'Etna (Nord, Sud, Est/Misto).
- **Tecnologie**: HTML5 semantico, Vanilla CSS moderno (Google Fonts *Inter*, dark theme hero, responsive design), JavaScript vanilla (zero framework), Leaflet.js per la mappa interattiva.
- **Vincolo Fondamentale**: Risorse 100% locali con percorsi relativi per utilizzo offline (`file:///...`).

---

## 📁 Struttura della Cartella di Progetto

```text
ETNA_2026/
├── etna26.html                  # Applicazione principale con mappa Leaflet, schede itinerari, caroselli e lightbox
├── mobile_preview.html          # Simulatore smartphone (iPhone 14/15 390x844) per test visivo rapido su desktop
├── task.md                      # Task list aggiornata con tutti i requisiti completati
├── walkthrough.md               # Questo documento di riepilogo e passaggio consegne
└── images/                      # Cartella di tutte le immagini locali
    ├── trekking ai crateri 2002 e monte nero/                           # 7 foto Opzione 1 (Versante Nord)
    ├── Rifugio Sapienza – Cratere del 2001 – Quota 2.500m (Terminale Funivia)/ # 7 foto Opzione 2 (Versante Sud)
    └── Sentiero Serracozzo (Valle del Bove)/                             # 11 foto Opzione 3 (Misto Serracozzo + Sartorius)
```

---

## 📸 Mappatura Completa delle Gallerie Fotografiche

Tutte le fotografie provengono direttamente dalle sottocartelle fornite dall'utente dentro `images/`, con percorsi relativi opportunamente URL-encoded per garantire compatibilità offline universale:

### 🌲 Opzione 1: Versante Nord (Piano Provenzana • Bottoniera 2002 • Monte Nero)
*Cartella: `images/trekking ai crateri 2002 e monte nero/` (7 foto totali)*
1. **Copertina & Slide 1**: `crateri-2002-vulcano-etna-2048x1536.jpg` – Panoramica aerea della bottoniera e di Monte Nero.
2. **Slide 2**: `contrasto-lava-vegetazione-eruzione-2002-1200x538.jpg` – Contrasto tra lave recenti e vegetazione pioniera.
3. **Slide 3**: `trekking-etna-nord-crateri-2002-900x1200.jpg` – Sentiero sui crinali e orlo della frattura vulcanica.
4. **Slide 4**: `camminata-crateri-del-2002-900x1200.jpg` – Escursionisti in quota verso 2.000m con vista sui Peloritani.
5. **Slide 5**: `anniek-etna-kraters-2002-e1526108275673-1024x576.jpg` – Sosta panoramica sull'orlo del cratere con affaccio sul mare.
6. **Slide 6**: `monte_nero_1.jpg` – Salita sui versanti scoriacei del cono antico di Monte Nero (2.049 m).
7. **Slide 7**: `monte_nero_2.jpg` – Rifugio in pietra lavica tra i pini e cima fumante dell'Etna.

### 🌋 Opzione 2: Versante Sud (Rifugio Sapienza • Cratere del 2001 • Quota 2.500m)
*Cartella: `images/Rifugio Sapienza – Cratere del 2001 – Quota 2.500m (Terminale Funivia)/` (7 foto totali)*
1. **Slide 1**: `rifugio_sapeinza.webp` – Piazzale di partenza a quota 1.900 m con il rifugio storico CAI.
2. **Copertina & Slide 2**: `etna_rifugio_sapienza.jpg` – Vista aerea sui Crateri Silvestri e sulla strada da Nicolosi.
3. **Slide 3**: `images-1.jpg` – Funivia dell'Etna risalente il deserto lavico verso quota 2.500m.
4. **Slide 4**: `cratere_2001_4.jpg` – Salita sul cono del 2001 con vista sul golfo di Catania.
5. **Slide 5**: `446561c26f10c7940c54171cdc971794.jpg` – Escursionisti verso il Cratere del Laghetto e la Montagnola.
6. **Slide 6**: `images.jpg` – Pendici scoriacee della Montagnola a 2.500 m.
7. **Slide 7**: `etna-crateri-sommitali-dal-rifugio-sapienza_1bbc.jpg` – Arrivo al terminale funivia con vista sulla cima attiva.

### 🍁 Opzione 3: Escursione Mista (Serracozzo • Valle del Bove • Monti Sartorius)
*Cartella: `images/Sentiero Serracozzo (Valle del Bove)/` (11 foto totali)*
1. **Slide 1**: `serracozzo-dal-rifugio-citelli_5f11.jpg` – Salita nel canalone di sabbia nera tra i cespugli di Spino Santo.
2. **Slide 2**: `serracozzo-dal-rifugio-citelli_e2e9.jpg` – Lucernario vulcanico con segnavia CAI e bosco di betulle.
3. **Slide 3**: `etna_881396783.jpg` – Interno spettacolare della galleria lavica a "buco di serratura".
4. **Slide 4**: `grotta.jpg` – Raggio di sole penetrante nella volta basaltica della grotta.
5. **Slide 5**: `Trekking-alla-Grotta-di-Serracozzo-e-Valle-del-Bove-4.webp` – Cresta di Serra delle Concazze sopra il mare di nuvole.
6. **Copertina & Slide 6**: `valle-del-bove-etna-2400x1350.jpg` – Spettacolare affaccio a picco sull'anfiteatro della Valle del Bove.
7. **Slide 7**: `valle-del-bove-vulcano-etna-2400x1350.jpg` – Camminata sui dicchi magmatici e pareti laviche primordiali.
8. **Slide 8**: `Etna-escursioni-didattiche-per-gli-alunni-con-le-guide-vulcanologiche-scaled.webp` – Bottoniera dei Monti Sartorius (1865) verso Taormina.
9. **Slide 9**: `Atna-Bildungsausfluge-fur-Schulen-scaled.webp` – Anello dei coni scoriacei punteggiati da pini dell'Etna.
10. **Slide 10**: `caption.jpg` – Orlo craterico del cono principale dei Sartorius.
11. **Slide 11**: `9b33f77c5c6cbb3d1708c688cd147408560b6bf8c6a48f4011a98667e11537e5.avif` – Panorama orientale verso il mar Ionio.

---

## ⚙️ Architettura dei Componenti e Logica JavaScript

1. **Mappa Interattiva (Leaflet)**:
   - Centrata sulla Sicilia orientale (`setInitialView()`).
   - Marker personalizzati con icone SVG nitide: Alloggio (rosso), P1 (Nord - blu), P2 (Sud - ambra), 3A/3B (Est - verde).
   - Tracciati stradali calcolati da OSRM ed evidenziati su selezione scheda o pulsante filtro.

2. **Selettori Itinerario (Filtri)**:
   - Funzione `filterSelection(routeId)`: evidenzia la card attiva, aggiorna la mappa e sincronizza lo stato visivo.
   - Su smartphone i pulsanti si trasformano in una barra swipe orizzontale touch (senza andare a capo).

3. **Caroselli Fotografici Dinamici**:
   - Gestiti da `moveCarousel(routeId, direction, event)` e `setCarouselSlide(routeId, index, event)`.
   - Il numero di slide è dinamico (`slides.length`): gestisce automaticamente sia i caroselli da 7 foto sia quello da 11 foto.
   - Indicatori dots sincronizzati con classe `.active`.
   - Contatore aggiornato in tempo reale (`1 / 7`, `1 / 11`, ecc.).

4. **Lightbox Modal a Schermo Intero**:
   - Funzioni: `openModal(src, title, desc, event, routeId, slideIndex)`, `closeModal()`, `navigateModal(direction, event)`.
   - Supporto scorciatoie da tastiera: `Escape` per chiudere, `ArrowLeft` per precedente, `ArrowRight` per successiva.
   - Sincronizzazione automatica tra il modal e la slide della card sottostante.
   - Adattivo su mobile (`max-height: 92vh` con overflow interno scrollabile).

---

## 📱 Ottimizzazione Mobile & Smartphone

- **Breakpoints**:
  - `@media (max-width: 900px)`: Mappa e pannello info incolonnati verticalmente, altezza mappa a 380px.
  - `@media (max-width: 768px)`: Layout a colonna singola per le schede itinerario (`1fr`), filtri swipe orizzontali touch-friendly con scrollbar invisibile, altezza mappa a 300px, altezza carosello a 190px, pulsanti touch 36x36px, modal lightbox scrollabile.
  - `@media (max-width: 400px)`: Micro-padding (10px) per smartphone compatti (iPhone SE / schermi 360px).
- **Simulatore Locale**:
  - È disponibile [`mobile_preview.html`](file:///Users/marco/Desktop/ETNA_2026/mobile_preview.html) per verificare in qualsiasi momento il comportamento su un viewport reale iPhone 14/15 (390 x 844 px).

---

## 🧪 Stato Verifiche

- **Errori JS console**: 0.
- **Caricamento immagini e percorsi relativi**: 100% verificati (0 codici 404).
- **Supporto formati moderni**: JPG, WebP e AVIF testati e visualizzati correttamente.
- **Accessibilità & leggibilità**: Contrasti conformi, font Inter ben gerarchizzato, touch targets ampi.

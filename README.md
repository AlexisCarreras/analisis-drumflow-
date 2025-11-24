# 🥁 DrumFlow – App para Bateristas (Metronome + Partituras + Live Mode)

Proyecto personal para crear una aplicación que ayude a los bateristas a estudiar, aprender y tocar en vivo canciones nuevas, integrando:

- Metrónomo avanzado.
- Partes de canciones (Intro, Estrofa, Puente, Estribillo).
- Letras sincronizadas.
- Partituras / patrónes rítmicos.
- Modo Live optimizado para escenario.
- Opcional: IA para generar o sugerir partituras.

---

# 📌 **1. Primer estudio de competencia ¿Qué tiene Drumeo, características, beneficios y qué le falta?**

## ✔️ Características principales de Drumeo:
- Reproductor de canciones integrado.
- Pistas drumless y full track.
- Partituras oficiales sincronizadas.
- Marcas de secciones: Intro, Verso, Puente, Solo, Outro.
- Control de tempo (½, ¾, 1x).
- Loop de secciones.
- Marcado de progreso.
- Recomendaciones de canciones.
- App mobile bien optimizada.

## 🎯 Beneficios para el músico:
- Aprendizaje estructurado.
- Mayor velocidad para aprender repertorios.
- Precisión rítmica y técnica.
- Ahorro de tiempo al estudiar.

## ❌ Puntos débiles y oportunidades de mejora:
- No combina letra + partitura + secciones + metrónomo al mismo tiempo.
- No tiene “Modo Live” minimalista.
- No tiene notas personales por sección.
- No se puede importar una canción con BPM automático.
- No integra IA para transcribir batería.
- La creación de partituras es compleja para usuarios no expertos.
- No está pensado para shows en vivo.

---

# 📌 **2. Análisis técnico del requerimiento**

Para satisfacer todo lo necesario, la app requiere:

### 🔊 **Audio Engine**
- Reproducción de música.
- Sincronización con un metrónomo.
- Loop de secciones.
- Cambios de tempo tiempo real  
  → **Tone.js** o **Web Audio API**

### 🎼 Partituras / Representación visual
Opciones:
- **VexFlow** (notación musical completa).
- **OpenSheetMusicDisplay** (MusicXML compatible).
- Editor propio tipo “blocks” orientado a batería.

### 📝 Gestión de canciones
- Archivos
- BPM
- Secciones
- Letras
- Partituras
- Notas personales
- Setlists

### ⚙️ Backend
- CRUD de canciones y secciones
- Procesamiento de audio (FFmpeg, BPM Detection)
- Integración opcional con IA

### 📱 UX Live
Un UI adaptado a shows:
- BPM grande
- Letra clara
- Barra que avanza
- Indicador de sección
- Cambios visibles a distancia

---

# 📌 **3. Análisis del stack actual**

### ✔️ Frontend
- **React + TypeScript** → estructura clara + mantenimiento
- **React Query** → caching ideal para audio + metadata
- **Zustand** → excelente para controlar el timeline global
- **Tailwind / MUI** → para vistas limpias y rápidas
- **Tone.js** → manejo profesional del audio

### ✔️ Backend
- **Node + Express** → ideal para APIs simples
- **MySQL o PostgreSQL** → relación Song ↔ Sections ↔ Partituras
- **Supabase / Firebase Storage** → hosting de archivos de audio

### ❗ Puntos claves
- Tone.js avanzado (Transport, Scheduler)
- VexFlow
- Web Audio API
- IA para detección de audio (BasicPitch, Onsets and Frames)

---

# 📌 **4. Módulos que debe tener la app**

## 1️⃣ **Song Library**
- Lista de canciones
- Filtros
- Favoritos
- Setlists
- BPM detection

## 2️⃣ **Song Viewer (modo estudio)**
- Partituras
- Letra
- Secciones
- Metronomo
- Player
- Loops

## 3️⃣ **Song Viewer (modo vivo)**
- Layout minimalista
- BPM grande
- Sección actual
- Letras tamaño escenario
- Cursor de avance

## 4️⃣ **Editor de Partituras**
Modos:
- Notación musical completa (VexFlow)
- Editor simple tipo blocks
- Editor estilo DAW/Piano Roll

## 5️⃣ **Importador de Canciones**
- Carga de audio (mp3/wav)
- Detección automática de BPM
- Marcar secciones

## 6️⃣ **IA (opcional pero alto impacto)**
- Transcripción básica de batería
- Conversión Audio → MIDI → Notación
- Sugerencia de sticking
- Detección automática de secciones

## 7️⃣ **Progreso**
- Canciones aprendidas
- Horas practicadas
- Estadísticas semanales

---

# 📌 **5. IA existente para generar partituras**

## ✔️ Audio → MIDI
- **Spotify Basic Pitch** (muy bueno y open source)
- **Magenta Onsets & Frames**
- **Essentia**
- **Melody.ml**

## ✔️ IA especializada en batería
- Modelos Magenta entrenados para percusión
- **BeatNet**: detección de kick/snare/hh
- Redes neuronales para Transcripción Drum Kit

## ✔️ IA comercial / APIs externas
- OpenAI Audio Models (Audio Understanding)
- Google ML Kit Audio

## ❗ Conclusión:
Sí se puede generar una **transcripción aproximada**, suficiente para ayuda visual.

---

# 📌 **6. Tecnologías recomendadas**

## ⭐ Obligatorias
- **Tone.js**
- **VexFlow**
- **Web Audio API**

## ⭐ Altamente recomendadas
- BasicPitch / Magenta
- FFmpeg (para procesamiento de audio)
- Capacitor (si orientamos a mobile nativo)
- Supabase Storage

---

# 📌 **7. Diagrama de Arquitectura Completo**

```
                       +-----------------------------+
                       |          FRONTEND           |
                       |  React + TS + Zustand        |
                       |  Tone.js (audio + metronome) |
                       |  VexFlow (partituras)        |
                       +--------------+--------------+
                                      |
                                      | REST / JSON
                                      v
                    +-----------------+-----------------+
                    |                BACKEND            |
                    |          Node + Express           |
                    |  Auth | Songs | Sections | Sheets |
                    |  BPM Detection (FFmpeg/Libs)      |
                    |  IA Gateway (Magenta / OpenAI)    |
                    +-----------------+-----------------+
                                      |
                                      v
                       +------------------------------+
                       |            STORAGE            |
                       | Supabase / Firebase / S3      |
                       | Audio Files | Images | PDFs    |
                       +------------------------------+
                                      |
                                      v
                          +------------------------+
                          |       DATABASE         |
                          |  MySQL / Postgres      |
                          +------------------------+
```

---

# 📌 **8. Estructura de Carpetas del Proyecto**

```
/drumflow-app
 ├─ /frontend
 │   ├─ /src
 │   │   ├─ /components
 │   │   ├─ /features
 │   │   │   ├─ songs/
 │   │   │   ├─ editor/
 │   │   │   ├─ player/
 │   │   ├─ /hooks
 │   │   ├─ /stores (Zustand)
 │   │   ├─ /services (React Query)
 │   │   ├─ /utils (Tone timeline)
 │   │   └─ main.tsx
 │   └─ index.html
 ├─ /backend
 │   ├─ /src
 │   │   ├─ /controllers
 │   │   ├─ /services
 │   │   ├─ /repositories
 │   │   ├─ /routes
 │   │   ├─ /middlewares
 │   │   ├─ /models
 │   │   └─ app.js
 │   └─ package.json
 ├─ /docs
 └─ README.md
```

---

# 📌 **9. Modelos de Base de Datos**

## `Song`
- id
- title
- artist
- bpm
- audioUrl
- createdAt
- updatedAt

## `SongSection`
- id
- songId
- name (Intro, Verse, Solo…)
- startTime
- endTime

## `SongSheet`
- id
- songId
- type (“vexflow”, “blocks”, “midi”)
- data (JSON)

## `SongLyrics`
- id
- songId
- text
- timecodes (JSON opcional)

## `Setlist`
- id
- name
- userId

## `SetlistSong`
- setlistId
- songId
- order

---

# 📌 **10. Endpoints del Backend**

### Songs
```
GET    /songs
GET    /songs/:id
POST   /songs
PUT    /songs/:id
DELETE /songs/:id
```

### Sections
```
GET    /songs/:songId/sections
POST   /songs/:songId/sections
PUT    /songs/:songId/sections/:id
DELETE /songs/:songId/sections/:id
```

### Sheet Music
```
GET    /songs/:songId/sheet
POST   /songs/:songId/sheet
PUT    /songs/:songId/sheet/:id
```

### IA
```
POST /ai/transcribe
POST /ai/detect-bpm
POST /ai/split-drums
```

---

# 📌 **11. Mocks de UI Inspirados competencias**

## 🟦 Song Viewer (Estudio)

```
+-------------------------------------------------------+
|  [Cover Art]  TITLE - ARTIST                           |
|--------------------------------------------------------|
|   ► Play | BPM: 96 | Tap Tempo | Loop Section          |
|--------------------------------------------------------|
|   Sections: Intro | Verse | Chorus | Solo | Outro      |
|--------------------------------------------------------|
|   [ PARTITURA / SCROLL AUTOMÁTICO ]                    |
|--------------------------------------------------------|
|   Lyrics + Timecodes                                   |
+-------------------------------------------------------+
```

## 🟨 Modo Live

```
+----------------------------------------------+
| BPM: 96                                       |
| SECTION: CHORUS                               |
|                                               |
|   When you were here before...               |
|   Couldn't look you in the eye...            |
|                                               |
|   [Pointer moving across timeline]           |
+----------------------------------------------+
```

---

# 📌 **12. Timeline + Sincronización con Tone.js**

### Timeline JSON sugerido
```json
{
  "bpm": 96,
  "sections": [
    { "name": "Intro", "start": 0, "end": 12.5 },
    { "name": "Verse", "start": 12.5, "end": 48 },
    { "name": "Chorus", "start": 48, "end": 72 }
  ],
  "lyrics": [
    { "time": 13.0, "text": "When you were here before" },
    { "time": 17.5, "text": "Couldn't look you in the eye" }
  ]
}
```

### Scheduler
```ts
Tone.Transport.scheduleRepeat((time) => {
  metronomeTick(time);
}, "4n");

Tone.Transport.schedule((time) => {
  highlightSection("Verse");
}, 12.5);
```

---

# 📌 **13. Próximos pasos:**
- Crear el boilerplate del proyecto
- Armar Song Viewer básico
- Implementar Tone.js con metronomo
- Cargar un audio y sincronizar timeline
- Renderizar partitura con VexFlow
- Crear modo Live
- Evaluar IA para transcripción

---


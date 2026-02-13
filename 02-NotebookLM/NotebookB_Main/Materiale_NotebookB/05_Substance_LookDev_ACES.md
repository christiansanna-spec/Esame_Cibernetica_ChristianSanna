# METADATA
# Software: Substance 3D Painter / Blender 5.0 / UE5.4+
# Modulo: LookDev & Shading
# Argomento: ACES 1.3 Workflow & Texture Packing
# Tipo Fonte: Studio Standard (Blender 5.0 Native Update)
# Data Parsing: 2026-01-10

# Substance to UE5: The ACES Color Workflow

L'obiettivo è garantire che i colori visti in Substance Painter siano identici a quelli in Blender 5.0 e Unreal Engine 5 (WYSIWYG).

## 1. La Teoria: Perché ACES?
Unreal Engine 5 usa **ACEScg** come spazio colore di rendering. Se lavori in sRGB standard in Substance, i tuoi colori appariranno sbiaditi e "sbagliati" in engine, specialmente i rossi e i blu intensi.
* **Regola:** Tutta la pipeline deve parlare la lingua ACES.

## 2. Setup Substance Painter (Obbligatorio)
Quando crei un nuovo progetto:
1.  **Color Management:** Seleziona **OpenColorIO**.
2.  **OCIO Config:** Carica il file config ACES 1.3 (scaricabile da [opencolorio.org](https://opencolorio.org)).
3.  **Bit Depth:** Imposta sempre a **16bit** per evitare il "banding" sulle normal map.

**Viewport Setting (Cruciale):**
* Vai nei settings della Viewport Display.
* Imposta **View Transform** su **ACES sRGB** (o *Un-tone-mapped* se vuoi vedere i valori raw, ma ACES sRGB simula il monitor finale).

## 3. Setup Blender 5.0 (Native ACES)
Grazie a Blender 5.0, non servono più config esterni.
1.  Vai in **Render Properties > Color Management**.
2.  **Display Device:** sRGB.
3.  **View Transform:** Seleziona **ACES** (Nuova opzione nativa in 5.0).
    * *Nota:* Non usare "AgX" se il target finale è Unreal, perché Unreal usa il Tonemapper ACES (Filmic) di default. Usare ACES in Blender ti dà la parità visiva 1:1.
4.  **Sequencer/Compositor:** Assicurati che lo spazio colore di lavoro sia impostato su **ACEScg** (Linear).

## 4. Texture Export Strategy (UE5 Packed)
Per ottimizzare la memoria in Nanite, le texture devono essere impacchettate (Packed).

**Creare un Output Template in Substance:**
Nominalo: `UE5_Nanite_Packed`
* **Base Color (RGB):** Canale sRGB.
* **Normal (RGB):** DirectX Format (Y-). *Unreal usa DirectX (Verde giù). Blender usa OpenGL (Verde su).*
* **ORM Packed (RGB):**
    * **R (Red):** Ambient Occlusion (Gray Channel)
    * **G (Green):** Roughness (Gray Channel)
    * **B (Blue):** Metallic (Gray Channel)

## 5. La "Trappola sRGB" in Unreal Engine
Questo è l'errore n.1 che distrugge il look dei materiali.

**Procedura di Import in UE5:**
1.  Importa le texture.
2.  Apri la texture **ORM** (Occlusion-Roughness-Metallic).
3.  **Togli la spunta a "sRGB"**.
    * *Perché:* Questi sono dati matematici (quanto è ruvido?), non colori per l'occhio umano. Se lasci sRGB attivo, Unreal applica una correzione gamma che "sbianca" la roughness, rendendo tutto lucido e plasticoso.
4.  Apri la texture **Normal Map**: Verifica che la compressione sia "Normalmap" (dovrebbe farlo in automatico).

## 6. Debugging Visivo
Se il materiale sembra diverso tra Blender 5.0 e UE5:
* Controlla l'HDRI. Stai usando la stessa mappa ambientale in entrambi?
* Controlla l'intensità della luce (Lux). Blender Cycles e Lumen gestiscono l'esposizione in modo simile ma non identico.
* **Verifica finale:** Importa la mesh in UE5, crea un materiale instance, assegna le texture. Se il metallo sembra nero, hai dimenticato di togliere sRGB dalla mappa ORM.
# METADATA
# Software: Blender 4.2+ / UE5.4
# Modulo: Pipeline & Interchange
# Argomento: USD Export & Data Sanitation
# Tipo Fonte: Studio Standard / Technical Workflow
# Data Parsing: 2026-01-10

# Blender to UE5: USD & Geometry Nodes Workflow

Questo documento definisce lo standard per il passaggio dati da Blender a Unreal Engine 5. L'obiettivo è ottenere asset puliti, leggeri e con la scala corretta senza intervento manuale in engine.

## 1. Il Problema della Scala (The Scale Fix)
Blender usa Metri. Unreal usa Centimetri. L'export standard FBX/USD spesso risulta in asset grandi 100 volte il dovuto o minuscoli.

**Procedura Obbligatoria:**
1.  **Scene Properties:** Lasciare `Unit Scale` a **1.000000** (Metri). *Non toccare questo valore globale o romperai la fisica.*
2.  **Export Settings (USD):**
    * Unreal Engine 5 converte automaticamente le unità USD se i metadati sono corretti, MA per sicurezza in produzione si usa il **"Global Scale 100"** trick se si riscontrano problemi.
    * **Best Practice:** Modellare in scala reale (1 cubo = 1 metro in Blender = 100 unità in Unreal).

**Nota Aggiornamento Blender 5.0:** Con il nuovo importatore FBX C++ di default in Blender 5.0, l'opzione "Apply Transform" è molto più stabile. Tuttavia, la regola della scala (Unit Scale 0.01) rimane la safest bet per evitare problemi con la fisica in UE5. Per il Color Management: Usare il preset nativo **ACES** invece di Filmic/AgX quando si lavora per UE5.
## 2. Geometry Nodes "Sanitizer"
Prima di esportare, ogni mesh deve passare attraverso questo modificatore per rimuovere dati "spazzatura" (come vertex groups inutili o attributi di velocità) che aumentano la dimensione del file e confondono Nanite.

**Setup del Nodo (Copia questo grafo):**

1.  Crea un nuovo Geometry Nodes Modifier chiamato `GN_Export_Clean`.
2.  Aggiungi i seguenti nodi in sequenza:
    * **Remove Named Attribute:** Imposta su *Wildcard* e scrivi `velocity*` (rimuove dati di velocità).
    * **Remove Named Attribute:** Aggiungi un altro nodo per rimuovere `temperature*` o `density*` (residui di scultura).
    * **Triangulate:** (Opzionale ma raccomandato) Assicura che la silhouette sia identica in UE5. Imposta su *Fixed* o *Shortest Diagonal*.
    * **Store Named Attribute:** (Opzionale) Se serve forzare un colore, usa questo nodo per scrivere su `Col`.
    * **Set Material:** Per assicurare che gli slot materiale siano assegnati correttamente.

> **Perché farlo?** Unreal Engine prova a importare *tutto*. Se esporti una mesh da 10M di poligoni con dati di "velocity" su ogni vertice, raddoppi la RAM necessaria in UE5 inutilmente.

## 3. Impostazioni Export USD (Preset: UE5_Nanite)
Nel menu **File > Export > Universal Scene Description (.usd)**:

* **Selection Only:** [x] **ON** (Evita di esportare camere, luci e oggetti nascosti).
* **Data > Materials:** [x] **ON** (Esporta i riferimenti ai materiali).
* **Data > UV Maps:** [x] **ON** (Rinomina automaticamente la mappa UV attiva in `st` o `UVMap`).
* **Experimental > Instancing:** [ ] **OFF** (Per Nanite, spesso è meglio avere mesh uniche o usare l'instancer di Unreal, a meno che non sia un layout di migliaia di oggetti identici).
* **Subdivision:** [ ] **OFF** (Lasciare OFF. Nanite non vuole geometry subdivisa proceduralmente all'export).

## 4. Bridge ZBrush -> Blender -> UE5
Se Blender è solo un passaggio intermedio per il Retopology:
1.  Importa FBX da ZBrush.
2.  Applica `GN_Export_Clean`.
3.  Verifica orientamento facce (Face Orientation - tutto Blu, niente Rosso).
4.  Esporta USD/FBX verso UE5.
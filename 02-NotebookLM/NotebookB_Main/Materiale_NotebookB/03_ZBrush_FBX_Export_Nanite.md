# METADATA
# Software: ZBrush 2024+
# Modulo: FBX ExportImport Plugin
# Argomento: Export Settings for Unreal Engine 5 (Nanite)
# Tipo Fonte: Technical Standard / Studio Pipeline
# Data Parsing: 2026-01-10

# ZBrush to UE5: FBX Export Guidelines

Quando si esportano mesh ad alta densità (High Poly) per Nanite, le impostazioni del plugin FBX ExportImport sono critiche per evitare file giganteschi e errori di shading.

## Posizione del Tool
Il pannello si trova in: **Zplugin > FBX ExportImport**.

## Configurazioni Obbligatorie (Nanite Ready)

### 1. Normals (SNormals vs Normals)
* **Setting:** Selezionare **SNormals** (Smooth Normals).
* **Motivazione Tecnica:**
    * Nanite preferisce normali morbide.
    * L'opzione standard "Normals" (Hard Normals) duplica i vertici su ogni bordo duro per creare lo shading netto. Questo aumenta drasticamente il conteggio dei vertici e la dimensione del file su disco.
    * **SNormals** esporta 1 normale per vertice, mantenendo il file più leggero e il parsing di Nanite più veloce.

### 2. Triangolazione
* **Setting:** Attivare **Triangulate**.
* **Motivazione Tecnica:**
    * ZBrush lavora nativamente con Quads, ma le GPU e Nanite lavorano con Triangoli.
    * Se non triangoli in esportazione, Unreal triangolerà all'importazione. L'algoritmo di triangolazione di Unreal potrebbe differire da quello di ZBrush, cambiando leggermente la silhouette o creando artefatti su superfici concave.
    * Triangolare in ZBrush garantisce la coerenza visiva 1:1 ("What You See Is What You Get").

### 3. Formato File
* **Setting:** **FBX 2020** (o superiore) in formato **Binario**.
* **Attenzione:** Evitare il formato ASCII (testo), produce file enormi e lenti da leggere per l'importer di UE5.

### 4. Gestione Colore (Polypaint)
* **Setting:** Attivare **Vertex Color** (se si usa Polypaint).
* **Workflow Note:** Se i colori non appaiono in UE5, assicurarsi che il materiale in Unreal abbia un nodo "Vertex Color" collegato alla Base Color.

## Checklist Pre-Export
Prima di cliccare "Export":
1.  Controllare che **Decimation Master** sia stato applicato (Target 2-5M triangoli).
2.  Controllare che **all Subtools** abbiano nomi univoci (UE5 unisce le mesh con nomi identici).
3.  Verificare la scala (Se non si usa il bridge: assicurarsi che la scala sia coerente con UE5, 1 unit = 1 cm).
# METADATA
# Argomento: Topology & Modeling Standards
# Tipo Fonte: Industry Best Practices (TopologyGuides / SubD Theory)
# Ruolo: Quality Control Guidelines

# Standard di Topologia e Modellazione

Questo documento definisce i criteri di accettazione per qualsiasi mesh poligonale prodotta nello studio. Il Supervisore deve rifiutare gli asset che non rispettano questi principi.

## 1. Filosofia Generale
La topologia non è estetica, è **funzione**.
* **Mesh Statica (Environment/Props):** La priorità è la silhouette e lo shading. I triangoli sono accettati su superfici piane.
* **Mesh Deformabile (Character/Creature):** La priorità è il flusso dei loop (Edge Flow). Solo QUADS.

## 2. Regole per Asset Deformabili (Characters)
Per visi, corpi e creature che dovranno essere riggati:

### A. I "Tre Cerchi" del Viso
Devono esistere loop concentrici puliti attorno a:
1.  **Occhi (Orbicolare):** Minimo 2 anelli di loop puliti per permettere la chiusura delle palpebre.
2.  **Bocca (Orbicolare):** Loop che circondano le labbra per fonemi (O, U).
3.  **Maso-Labiale:** Un loop che parte dal naso e circonda la bocca (fondamentale per il sorriso).

### B. Gestione dei Poli (Poles)
* **Polo a 3 (E-Pole):** Accettabile per curvare il flusso, ma MAI su zone di forte deformazione (es. angolo bocca).
* **Polo a 5 (N-Pole):** Necessario per cambiare direzione dei loop. Va nascosto in zone di bassa deformazione (es. tempie, sotto il mento).
* **Polo a 6+ (Stella):** **VIETATO.** Causa pizzicamenti (pinching) visibili nello shading. Deve essere risolto aggiungendo geometria.

### C. N-Gons (Poligoni > 4 lati)
* **Status:** **VIETATO** su mesh deformabili.
* **Perché:** Gli engine calcolano la triangolazione in tempo reale. Un N-Gon si piega in modo imprevedibile durante l'animazione, creando artefatti neri.

## 3. Regole per Hard Surface (Props/Mecha)
Con l'avvento di Nanite e il Weighted Normal workflow:

* **Support Loops:** Non sono più strettamente necessari per tenere i bordi se si usa un workflow "Mid-Poly" con *Weighted Normals* o *Auto Smooth*.
* **Cilindri:** Devono avere abbastanza lati per mantenere la silhouette rotonda anche a distanza (es. minimo 16 o 32 lati). Non affidarsi solo allo smoothing shader.
* **Boolean:** L'uso di Booleane è permesso, a patto che:
    1. Si applichi un modificatore *Weighted Normal* dopo.
    2. Non ci siano vertici sovrapposti (Merge by Distance obbligatorio).

## 4. Checklist di Controllo Qualità (QC)
Prima di passare alla fase UV o Sculpt, verifica:
1.  [x] **Lamina Faces:** Nessuna faccia sovrapposta.
2.  [x] **Non-Manifold Geometry:** La mesh deve essere "chiusa" e solida (niente bordi aperti inattesi).
3.  [x] **Face Orientation:** Tutte le normali puntano fuori (Tutto Blu in Blender).
4.  [x] **Scale:** Scala applicata (1,1,1).
# ARCHITETTURA CIBERNETICA DI LTX-2 (IL MOTORE SOTTO LTX STUDIO)

## 1. Il Processo a Due Stadi (The Two-Stage Pipeline)
A differenza di altri modelli, LTX non genera il video in un colpo solo. Usa un processo cibernetico a raffinamento progressivo:
* **Stadio 1 (Generation):** Il modello crea una versione a bassa risoluzione basata sul testo (Input) e sui vincoli temporali. Qui si definisce la struttura grezza.
* **Stadio 2 (Upsampling & Refinement):** Il sistema riprende l'output dello stadio 1 e applica un "spatial upsampler". È un loop di feedback che corregge i dettagli e aumenta la risoluzione senza allucinare nuovi oggetti.

## 2. Il Concetto di "Guida Multimodale" (Multimodal Guidance)
Il sistema riceve input contrastanti che deve armonizzare. Usa dei "Guider" per sterzare la generazione:
* **CFG (Text Guidance):** Quanto rigidamente il video deve obbedire al testo scritto.
* **STG (Spatio-Temporal Guidance):** Un controllo specifico per evitare che gli oggetti "tremino" o cambino forma nel tempo (coerenza temporale).
* **Audio-Video Coherence:** Esiste un parametro (`modality_scale`) che forza il video a sincronizzarsi con l'audio generato, riducendo l'entropia tra suono e immagine.

## 3. Metodi di Condizionamento (Input Control)
Come facciamo a imporre la nostra volontà (Scerbanenco) al sistema?
* **Replacing Latents:** Per mantenere i personaggi (Guido e Maria) uguali, il sistema non rigenera ogni frame da zero, ma "sostituisce i latenti" usando l'immagine di riferimento del personaggio. È come un ancoraggio che impedisce al volto di mutare.
* **Keyframe Interpolation:** Se diamo l'inizio e la fine della scena, il sistema calcola gli stati intermedi (frame) per collegarli fluidamente.
# Scene Recognition con Reti Neurali Convoluzionali (CNN)

## Descrizione del Progetto
Questo progetto affronta un problema di classificazione supervisionata di immagini per il riconoscimento di scene (Scene Recognition). L'obiettivo è catalogare le immagini in 15 categorie semantiche eterogenee (es. ufficio, cucina, autostrada, costa, foresta, ecc.) estratte dal dataset di riferimento *Lazebnik et al., 2006*.

Il lavoro è strutturato come un percorso evolutivo in tre fasi, esplorando e risolvendo progressivamente le criticità tipiche dell'addestramento di reti neurali, dall'overfitting su piccoli dataset all'estrazione di feature avanzate tramite modelli pre-addestrati.

## Struttura del Lavoro

### Fase 1: Baseline Shallow CNN
* **Architettura:** Creazione da zero di una CNN superficiale (3 blocchi convoluzionali con filtri 3x3, seguiti da ReLU e Max Pooling 2x2, e un classificatore Fully Connected).
* **Pre-processing:** Immagini convertite in scala di grigi e sottoposte a rescaling anisotropico a 64x64 pixel. Assenza di normalizzazione standard per preservare il range originale [0, 255].
* **Risultato:** Raggiungimento del target di baseline con un'accuratezza del **31.76%**. Evidente fenomeno di overfitting a causa dell'assenza di regolarizzazione.

### Fase 2: Ottimizzazione e Regolarizzazione
* **Architettura:** Potenziamento della rete base tramite raddoppio dei filtri convoluzionali (16 -> 32 -> 64). Introduzione di `Batch Normalization` prima delle attivazioni ReLU e di un livello di `Dropout (p=0.5)` prima del classificatore.
* **Pre-processing:** Arricchimento del training set tramite Data Augmentation (`RandomHorizontalFlip`).
* **Ottimizzazione:** Transizione dall'ottimizzatore SGD ad Adam.
* **Risultato:** Mitigazione della divergenza della Validation Loss e raddoppio delle performance, raggiungendo un'accuratezza del **59.93%**.

### Fase 3: Transfer Learning con AlexNet
* **Architettura:** Adozione della rete profonda pre-addestrata `AlexNet` (su dataset ImageNet). Adattamento dell'input a immagini RGB 224x224 con normalizzazione statistica standard.
* **Approccio A (Fine-Tuning):** Congelamento dei pesi estrattori di feature (`requires_grad = False`) e addestramento ex novo dell'ultimo livello lineare sulle 15 classi.
* **Approccio B (Feature Extraction & SVM):** Utilizzo della rete pre-addestrata come puro estrattore statico. Mappatura delle immagini in vettori a 4096 dimensioni e successiva classificazione tramite una Support Vector Machine (SVM) lineare multiclasse.
* **Risultato:** Entrambi gli approcci hanno superato ampiamente l'obiettivo progettuale dell'85%, con la SVM che ha registrato l'accuratezza massima dell'**86.23%**.

## Riepilogo Risultati

| Fase | Descrizione Modello | Test Accuracy | Note |
| :--- | :--- | :--- | :--- |
| **Fase 1** | Baseline Shallow CNN | 31.76% | Overfitting severo nelle epoche finali (early stopping applicato). |
| **Fase 2** | CNN + Augmentation + BatchNorm + Dropout | 59.93% | Ottima stabilizzazione e raddoppio della capacità predittiva. |
| **Fase 3A**| Transfer Learning: AlexNet Fine-Tuning | 86.06% | Convergenza in sole 10 epoche. |
| **Fase 3B**| Transfer Learning: AlexNet + SVM Lineare | **86.23%** | Miglior risultato complessivo. |

## Tecnologie Utilizzate
* **Linguaggio:** Python
* **Framework Deep Learning:** PyTorch (`torch`, `torch.nn`, `torchvision`)
* **Machine Learning & Metriche:** Scikit-learn (`LinearSVC`, `confusion_matrix`, `accuracy_score`)
* **Data Visualization:** Matplotlib, Seaborn
* **Ambiente di sviluppo:** Google Colab

---

## Contenuto della Repository
* **[Computer_Vision_and_Pattern_Recognition_PROJECT](./Computer_Vision_progetto_Di_Santo_Alessandro.ipynb)**: progetto descritto di cui sopra realizzato in linguaggio Python in Google Colab sfruttando un dataset di riferimento *Lazebnik et al., 2006* (caricato sul mio dropbox personale).
* **[Report_Progetto](./report_progetto_Di_Santo_Alessandro.pdf)**: report del progetto in formato pdf in cui vado a spiegare, con riferimenti teorici, "cosa" e "come" viene realizzato il progetto/codice di *Scene Recognition*.

---
*Progetto realizzato da: Alessandro Di Santo*  
*Professore: Felice Andrea Pellegrino*  
*Corso: Computer Vision and Pattern Recognition*  
*Computer Engineering — Università degli Studi di Trieste*


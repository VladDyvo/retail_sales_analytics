# Retail Sales Analytics - Online Retail II

Questo progetto si occupa dell'analisi avanzata dei dati di vendita di un e-commerce globale, coprendo l'intero biennio dal 2009 al 2011. L'obiettivo è trasformare oltre 1 milione di transazioni grezze in informazioni strategiche per il business attraverso tecniche di Data Cleaning, Exploratory Data Analysis (EDA), calcolo dei KPI commerciali, Analisi delle Coorti e segmentazione dei clienti tramite modello RFM.

---
## 📝 Stato Avanzamento Lavori

### 🧼 Fase 1: Ispezione Iniziale e Data Cleaning (Completato ✅)
Nel notebook `01_data_cleaning.ipynb` è stata eseguita la prima sfoltitura del dataset:
- **Unione del Dataset:** Uniti i dati del biennio ottenendo una base di partenza di **1.067.371 righe**.
- **Gestione dei Valori Nulli:** Il **22.77%** delle transazioni totali è privo di `Customer ID` (acquisti da utenti anonimi). Rilevate anche 4.382 descrizioni mancanti.
- **Rimozione Duplicati:** Individuate e rimosse **34.335 righe completamente duplicate**, portando il dataset finale a **1.033.036 righe** pulite.
- **Analisi Geografica:** Oltre il **91% del mercato** si concentra nel Regno Unito (`United Kingdom`), seguito da `EIRE` (Irlanda) e Germania.

---
 

### 🔄 Fase 1: Task 2 - Isolamento Resi e Flag Clienti Anonimi (Completato ✅)

* **Isolamento Resi:** Identificate **19.104 transazioni stornate** (Invoice con prefisso "C"), pari all'1.85% del totale, separate in un dataset `df_returns` dedicato.
* **Dataset Vendite:** Il dataset principale `df_sales` contiene **1.013.932 righe** di vendite effettive.
* **Clienti Anonimi:** Il **23.12% delle vendite (234.437 righe)** proviene da acquisti senza CustomerID, flaggati con la colonna `is_anonymous` per essere esclusi dall'analisi RFM ma mantenuti nei KPI aggregati.


---
### 🚀 Fase 1: Task 3 - Outlier Elimination e Feature Engineering (Completato ✅)

* **Analisi dell'Impatto per Variabile:** Per validare la scelta del metodo IQR, è stato verificato singolarmente l'impatto di ciascuna soglia: il filtro su `Price` (>8.45) esclude il 7.79% delle transazioni, mentre quello su `Quantity` (>28.5) esclude il 5.29%. La sovrapposizione tra i due filtri spiega la quasi totalità del 13.6% di righe eliminate complessivamente, confermando l'assenza di una singola soglia eccessivamente aggressiva.
* **Impatto Globale della Pulizia:**
  * **Righe iniziali (`df_sales`):** 1.013.932
  * **Righe finali (`df_cleaned`):** 876.436
  * **Righe eliminate:** 137.496 transazioni anomale complessive rimosse mediante l'applicazione congiunta dei filtri IQR.
* **Rimozione degli Outlier (Metodo IQR):** Le anomalie sono state rimosse dalle colonne `Quantity` e `Price` utilizzando i limiti dell'Intervallo Interquartile (1.5 × IQR):
  * **Quantity:** mantenuti i valori nel range **[1, 28.5]**
  * **Price:** mantenuti i valori nel range **[0.001, 8.45]**
* **Feature Engineering (`TotalPrice`):** Creata la nuova variabile `TotalPrice`, calcolata come `Quantity × Price`, per rappresentare il valore economico di ogni transazione. Dopo la pulizia, il valore massimo registrato è pari a **223.83**.
* **Dataset Finale:** Il dataset pulito è stato esportato correttamente nel file `output/dati_puliti/cleaned_retail.csv`, pronto per le successive analisi esplorative (EDA).

### 📊 Fase2 Analisi della Distribuzione Geografica del Fatturato

L'analisi geografica è stata condotta applicando un approccio a due livelli (Subplots) per gestire l'estrema asimmetria distributiva del dataset ed evitare l'effetto schiacciamento della scala visiva.

#### 🌍 Considerazioni di Business:
* **Dominanza del Mercato Interno (United Kingdom):** Come evidenziato nel grafico di sinistra (*Proporzione Globale*), il Regno Unito rappresenta il cuore pulsante del business, generando da solo oltre **9,16 milioni €** di fatturato. Questa cifra supera di gran lunga la somma di tutti gli altri mercati internazionali messi insieme.
* **I Top Player Esteri (No-UK):** L'isolamento strategico dell'UK nel grafico di destra (*Top 10 Mercati Esteri*) permette di analizzare accuratamente le performance internazionali:
  * L'**EIRE (Irlanda)** si conferma ufficialmente come il **secondo mercato aziendale più importante** in assoluto, con un fatturato di **275.030 €**.
  * La **Germania** segue a brevissima distanza in terza posizione con **266.907 €**, evidenziando un testa a testa commerciale molto competitivo con l'Irlanda.
  * La **Francia** solida al quarto posto con **206.820 €**, completando il trio dei mercati chiave europei.
  * A partire dalla **Svizzera** (53.805 €) in poi, si nota un netto gradino e un forte decremento del fatturato generato dagli altri paesi europei ed extra-europei.

#### 🛠️ Nota Tecnica di Data Science:
La visualizzazione affiancata dimostra l'importanza del preprocessing visivo. Se avessimo incluso il Regno Unito nello stesso grafico degli altri paesi, le barre di mercati importanti come EIRE, Germania e Francia sarebbero risultate microscopicamente insignificanti, impedendo l'estrazione di questi fondamentali insight strategici.

## 📁 Struttura del Progetto

```text
retail_sales_analytics/
├── .venv/                  # Ambiente virtuale Python
├── data/                   # Dataset originale (online_retail_II.xlsx)
├── output/                 # Grafici ed esportazioni finali
├── 01_data_cleaning.ipynb  # Pulizia dati e gestione anomalie
├── 02_eda.ipynb            # Analisi esplorativa e trend di vendita
├── 03_kpi_calculations.ipynb# Calcolo delle metriche di business (Fatturato, AOV, ecc.)
├── 04_rfm_and_cohorts.ipynb# Analisi della Retention (Coorti) e Segmentazione RFM
├── requirements.txt        # Librerie necessarie per il progetto
└── README.md               # Documentazione del progetto

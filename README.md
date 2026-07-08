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

---
### 📊 Fase 2: Task 1 - Analisi della Distribuzione Geografica del Fatturato

L'analisi geografica è stata condotta applicando un approccio a due livelli (Subplots) per gestire l'estrema asimmetria distributiva del dataset ed evitare l'effetto schiacciamento della scala visiva.

#### 🌍 Considerazioni di Business:
* **Dominanza del Mercato Interno (United Kingdom):** Come evidenziato nel grafico di sinistra (*Proporzione Globale*), il Regno Unito rappresenta il cuore pulsante del business, generando da solo oltre **9,16 milioni £** di fatturato. Questa cifra supera di gran lunga la somma di tutti gli altri mercati internazionali messi insieme.
* **I Top Player Esteri (No-UK):** L'isolamento strategico dell'UK nel grafico di destra (*Top 10 Mercati Esteri*) permette di analizzare accuratamente le performance internazionali:
  * L'**EIRE (Irlanda)** si conferma ufficialmente come il **secondo mercato aziendale più importante** in assoluto, con un fatturato di **275.030 £**.
  * La **Germania** segue a brevissima distanza in terza posizione con **266.907 £**, evidenziando un testa a testa commerciale molto competitivo con l'Irlanda.
  * La **Francia** solida al quarto posto con **206.820 £**, completando il trio dei mercati chiave europei.
  * A partire dalla **Svizzera** (53.805 £) in poi, si nota un netto gradino e un forte decremento del fatturato generato dagli altri paesi europei ed extra-europei.

#### 🛠️ Nota Tecnica:
La visualizzazione affiancata dimostra l'importanza del preprocessing visivo. Se avessimo incluso il Regno Unito nello stesso grafico degli altri paesi, le barre di mercati importanti come EIRE, Germania e Francia sarebbero risultate microscopicamente insignificanti, impedendo l'estrazione di questi fondamentali insight strategici.

---
### 📈 Task 2. Trend Mensile delle Vendite

L'analisi temporale è stata eseguita aggregando il fatturato su base mensile per osservare l'evoluzione delle vendite nel tempo. Per evidenziare il trend generale e attenuare le oscillazioni di breve periodo è stata calcolata una media mobile a 3 mesi.

#### 🔍 Considerazioni di Business

- Il fatturato mostra una chiara componente stagionale durante il periodo analizzato.
- I valori massimi vengono raggiunti nei mesi di **novembre 2010** e **novembre 2011**, suggerendo un significativo incremento della domanda nel periodo che precede le festività natalizie.
- Dopo ciascun picco si osserva una marcata contrazione delle vendite, indicando un comportamento ciclico tipico delle attività retail.
- Al di fuori dei picchi stagionali, il fatturato rimane relativamente stabile, oscillando tra circa **300.000 £** e **400.000 £** mensili.
- La media mobile a 3 mesi evidenzia una crescita progressiva del fatturato nel secondo semestre di ciascun anno, culminando nei picchi registrati a novembre.

> **Nota:** Il valore registrato a dicembre 2011 risulta sensibilmente inferiore rispetto agli altri mesi poiché il dataset termina il **9 dicembre 2011**, rappresentando quindi un mese incompleto e non direttamente confrontabile con gli altri periodi.

#### 🛠️ Nota Tecnica:

La colonna `InvoiceDate` è stata convertita nel formato `datetime` per consentire l'aggregazione temporale mediante `pd.Grouper(freq='ME')`, che raggruppa automaticamente le transazioni su base mensile. Successivamente è stata calcolata una media mobile a 3 mesi (`rolling(window=3).mean()`), utilizzata per smussare le fluttuazioni mensili ed evidenziare il trend di lungo periodo.

#### 📌 Insight Principale

L'azienda presenta una forte stagionalità delle vendite, con una crescita significativa del fatturato nel periodo che precede le festività natalizie. Questo comportamento suggerisce che campagne promozionali, gestione delle scorte e pianificazione delle risorse dovrebbero essere concentrate soprattutto nell'ultimo trimestre dell'anno, periodo in cui si registra il maggiore volume di ricavi.

---

### 🛍️ Task 3. Top 10 Prodotti per Volume e Fatturato

Per identificare gli articoli più rilevanti del catalogo sono state effettuate due analisi complementari:

- Top 10 prodotti per quantità venduta.
- Top 10 prodotti per fatturato generato.

Entrambe le classifiche sono rappresentate mediante grafici a barre orizzontali, facilitando il confronto tra volume di vendita e valore economico.

#### 🔍 Considerazioni di Business

- Il prodotto **WHITE HANGING HEART T-LIGHT HOLDER** occupa la prima posizione sia per quantità venduta sia per fatturato.
- I prodotti più richiesti appartengono prevalentemente alla categoria degli articoli decorativi e da regalo, coerentemente con il settore retail del dataset.
- Il confronto tra le due classifiche evidenzia che i prodotti con il maggior volume di vendita non coincidono sempre con quelli che generano il fatturato più elevato.
- Questa analisi consente di individuare gli articoli più importanti sia in termini di domanda sia di valore economico, fornendo indicazioni utili per la gestione dell'assortimento e delle strategie commerciali.

#### 🛠️ Nota Tecnica: 

Le classifiche sono state ottenute raggruppando il dataset per `Description`.

- Per il volume di vendita è stata calcolata la somma della colonna `Quantity`.
- Per il fatturato è stata calcolata la somma della colonna `TotalPrice`.

Successivamente sono stati selezionati i primi 10 prodotti ordinati in ordine decrescente e rappresentati mediante grafici a barre orizzontali.

---

### ⏰ Task 4. Analisi Oraria delle Vendite

Per comprendere come il fatturato si distribuisce nell'arco della giornata e individuare le fasce orarie di maggiore attività commerciale, è stato analizzato il fatturato complessivo in funzione dell'ora di emissione degli ordini.

#### 🔍 Considerazioni di Business

- **Fascia di maggiore attività:** Il fatturato si concentra prevalentemente tra le **10:00 e le 15:00**, intervallo in cui si registra la maggior parte delle vendite.
- **Picco giornaliero:** L'ora più redditizia è quella delle **12:00**, con un fatturato superiore a **1,5 milioni di £**, suggerendo che la tarda mattinata rappresenti il momento di massima attività dell'e-commerce.
- **Riduzione nel pomeriggio:** Dopo le **15:00** il fatturato diminuisce progressivamente, con un calo particolarmente evidente dopo le **17:00**, quando il numero di acquisti si riduce sensibilmente.

#### 🛠️ Nota Tecnica:

L'analisi è stata realizzata estraendo la componente oraria dalla colonna `InvoiceDate` mediante l'attributo `.dt.hour`, creando la nuova variabile `Hour`.

Successivamente il fatturato (`TotalPrice`) è stato aggregato per ciascuna ora della giornata tramite `groupby('Hour')` e rappresentato con un grafico a linee (`sns.lineplot`), una scelta che consente di evidenziare in modo efficace l'evoluzione temporale del fatturato durante la giornata.

#### 📌 Insight Principale

Le vendite risultano fortemente concentrate nelle ore centrali della giornata. Questa informazione può supportare decisioni relative alla pianificazione delle campagne marketing, alla gestione delle risorse operative e al dimensionamento dei servizi nei momenti di maggiore traffico.

---

### 📊 5. Analisi per Giorno della Settimana

Per identificare le dinamiche di acquisto settimanali e comprendere la distribuzione del volume d'affari, è stato analizzato il fatturato complessivo in funzione del giorno di emissione degli ordini.

#### 🔍 Considerazioni di Business

- **Concentrazione nei giorni feriali:** il fatturato si concentra prevalentemente nei giorni lavorativi, con il valore massimo registrato il **giovedì** (2.094.319 £).
- **Vendite minime nel fine settimana:** il minimo assoluto si registra il **sabato** (7.687 £), giorno in cui le vendite risultano quasi del tutto assenti.
- **Ripresa della domenica:** la **domenica** mostra un fatturato significativo (1.195.496 £), nettamente superiore al sabato e paragonabile ad alcuni giorni lavorativi.

#### 🛠️ Nota Tecnica

L'analisi è stata realizzata estraendo l'indice numerico del giorno della settimana dalla colonna `InvoiceDate` tramite l'attributo `.dt.dayofweek`.

Successivamente il fatturato (`TotalPrice`) è stato aggregato mediante `groupby()` e i valori numerici (0-6) sono stati convertiti nelle corrispondenti etichette in italiano tramite un dizionario di mapping. I risultati sono stati rappresentati con un grafico a barre (`sns.barplot`), mantenendo l'ordine cronologico dei giorni della settimana.

#### 📌 Insight Principale

Il comportamento osservato suggerisce una prevalente operatività **Business-to-Business (B2B)**, nella quale gli ordini tendono a concentrarsi durante la settimana lavorativa, mentre il sabato presenta un'attività commerciale pressoché nulla.

La distribuzione osservata e la ripresa delle vendite nella giornata di domenica potrebbero essere legate alle caratteristiche operative del business, come l'elaborazione degli ordini nel fine settimana o specifiche modalità di registrazione delle transazioni. Tali ipotesi richiederebbero ulteriori approfondimenti per essere confermate.

---
## 📊 Fase 3. Calcolo dei KPI di Business e Distribuzione degli Scontrini

Per ottenere una panoramica delle performance commerciali dell'e-commerce, sono stati calcolati i principali indicatori di business (KPI) e analizzata la distribuzione del valore degli ordini sul dataset pulito.

### 📈 KPI calcolati

- **Fatturato Totale Complessivo:** **10.405.563,73 £**
- **Numero Unico di Scontrini (Invoice):** **36.374**
- **Numero Unico di Clienti:** **5.682**
- **Scontrino Medio (AOV - Average Order Value):** **286,07 £**

La distribuzione del valore degli ordini è stata rappresentata tramite un istogramma con curva di densità (KDE), evidenziando la posizione dello scontrino medio rispetto all'intera distribuzione delle transazioni.

### 🔍 Considerazioni di Business

- **Distribuzione asimmetrica degli ordini:** La maggior parte degli scontrini presenta un valore relativamente contenuto, concentrandosi principalmente tra **50 £ e 200 £**, mentre il numero di ordini diminuisce progressivamente all'aumentare dell'importo.
- **Presenza di ordini ad alto valore:** La lunga coda della distribuzione evidenzia l'esistenza di un numero limitato di ordini di importo molto elevato, compatibili con clienti ad alto spendimento o acquisti all'ingrosso (B2B). Questi ordini influenzano significativamente il valore medio dello scontrino.
- **Frequenza media di acquisto:** Il rapporto tra **36.374 ordini** e **5.682 clienti unici** corrisponde a una media di circa **6,4 ordini per cliente**. Questo rappresenta un primo indicatore di una buona frequenza di acquisto, che verrà approfondito nelle successive analisi **RFM** e **Cohort Analysis**.

### 🛠️ Nota Tecnica

Le metriche sono state calcolate nel notebook `03_kpi_calculations.ipynb`.

- Il valore di ciascun ordine è stato ottenuto aggregando il fatturato (`TotalPrice`) per numero di fattura (`Invoice`) tramite `groupby()`.
- La distribuzione degli scontrini è stata visualizzata mediante un istogramma con curva di densità (KDE) utilizzando Seaborn.
- Per migliorare la leggibilità del grafico ed evitare che pochi ordini di importo molto elevato comprimessero la distribuzione, la visualizzazione è stata limitata agli scontrini inferiori a **1.000 £**. Tutti i KPI sono stati comunque calcolati sull'intero dataset.
- Il grafico finale è stato salvato nel percorso `output/grafici/distribuzione_scontrini.png`.    

---

## 📊 Fase 4 — Segmentazione RFM & Analisi delle Coorti

In questa fase il focus si è spostato dall'andamento macroeconomico del business (Fase 3) al comportamento microscopico del parco clienti, implementando due delle tecniche analitiche più potenti per il marketing e la fidelizzazione: la segmentazione RFM e l'analisi della Retention tramite Coorti.

### 🎯 Task 1: Segmentazione RFM (Recency, Frequency, Monetary)
Per comprendere il valore e lo stato di salute di ogni singolo cliente, è stato calcolato lo score RFM basato su tre metriche:
* **Recency (R):** Giorni trascorsi dall'ultimo acquisto rispetto a una data di riferimento globale (`max(InvoiceDate) + 1 giorno`).
* **Frequency (F):** Numero totale di transazioni uniche effettuate dal cliente.
* **Monetary (M):** Totale della spesa monetaria generata dal cliente.

#### 🛠️ Scelte Metodologiche & Ottimizzazioni:
* **Gestione Clienti Anonimi:** Come definito nella strategia, i calcoli RFM sono stati applicati esclusivamente sui clienti registrati con un ID valido (`Customer ID` non nullo), isolando 5.682 utenti unici senza alterare l'integrità del dataset principale.
* **Risoluzione Duplicati nei Quintili:** Per la metrica *Frequency*, la presenza di forti addensamenti di valori identici avrebbe causato errori nel calcolo dei quintili tramite `pd.qcut()`. Il problema è stato risolto applicando il metodo `.rank(method='first')`, garantendo una distribuzione omogenea e priva di sovrapposizioni.
* **Mappatura Avanzata:** I clienti sono stati segmentati in 10 categorie di business (es. *Champions*, *Loyal Customers*, *Can't Lose Them*, *Hibernating*) sfruttando la flessibilità delle espressioni regolari (`regex=True`) applicate sulle combinazioni strutturate di punteggi. La distribuzione finale ha registrato **0 clienti non mappati (NaN)**, a conferma della solidità del dizionario logico.

L'output finale è stato salvato in modo strutturato nel percorso tracciato dal repository: `output/dati_puliti/rfm_output.csv`.

---

### 🔄 Task 2: Analisi delle Coorti & Retention Rate
Per misurare la capacità del brand di trattenere i clienti nel tempo, è stata sviluppata un'analisi delle coorti basata sul mese del primo acquisto (*Mese di Coorte*).

#### 🛠️ Implementazione Tecnica:
1. **Identificazione della Coorte:** È stato isolato il mese della prima transazione assoluta di ciascun utente tramite un raggruppamento ottimizzato: `df_rfm.groupby('Customer ID')['InvoiceDate'].transform('min')`.
2. **Calcolo del CohortIndex:** È stata calcolata la distanza temporale in mesi interi tra il mese di ogni acquisto successivo e il mese di origine della coorte (`TransactionMonth - CohortMonth`), normalizzando i periodi con `.apply(lambda x: x.n)`.
3. **Generazione Matrice di Retention:** I dati sono stati aggregati in una tabella pivot di clienti unici e successivamente convertiti in percentuali dividendo ogni valore per la dimensione iniziale della rispettiva coorte (Mese 0).

#### 📈 Key Insights dal Grafico (`output/grafici/cohort_retention_heatmap.png`):
* **Churn immediato:** Tutte le coorti mostrano un calo drastico al mese 1 — in media solo il **20-35%** dei clienti effettua un secondo acquisto il mese successivo al primo. Questo indica un'opportunità critica di miglioramento nella strategia di onboarding e retention immediata.
* **Fidelizzazione della Coorte Storica:** La coorte iniziale di `2009-12` si dimostra la più solida del dataset, mantenendo un Retention Rate compreso stabilmente tra il **20% e il 49%** nei mesi successivi, mostrando una forte resistenza al logorio temporale.
* **Effetto Stagionalità (Natale):** Il grafico mostra una marcata diagonale di incremento dei riacquisti in corrispondenza di **Dicembre 2010**. I clienti storici accumulati nei mesi precedenti tendono a riattivarsi simultaneamente (ad esempio, la coorte `2010-01` risale al **40%** al mese 11 dopo essere scesa fino al 17% nei periodi precedenti), guidati dalla stagionalità degli acquisti festivi.
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

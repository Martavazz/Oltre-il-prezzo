# Oltre il prezzo: cosa dicono (davvero) i dati su Fast Fashion e Luxury

## Descrizione del progetto

**Oltre il prezzo** è un progetto di analisi esplorativa che mette a confronto i segmenti **Fast Fashion** e **Luxury** attraverso tre dimensioni: prezzo dei prodotti, linguaggio delle descrizioni dei brand e sentiment delle recensioni degli utenti.

L'analisi è stata realizzata in **Google Colab**, utilizzando notebook eseguibili in linguaggio Python, e combina tecniche di elaborazione del linguaggio naturale (NLP), analisi descrittiva, sentiment analysis e clustering. L'obiettivo è osservare se le differenze di posizionamento commerciale emergano anche dalle parole utilizzate e dalla percezione espressa nelle recensioni.

## Obiettivi e domanda di ricerca

**Il prezzo e il posizionamento Fast Fashion/Luxury si riflettono nel linguaggio delle descrizioni e nel sentiment delle recensioni?**

L'analisi si concentra su tre obiettivi:

1. **Confrontare i prezzi** dei due segmenti, osservando media e mediana.
2. **Individuare le caratteristiche lessicali** delle descrizioni dei prodotti attraverso TF-IDF.
3. **Esplorare il sentiment** delle recensioni, la sua relazione con il prezzo e la presenza di gruppi linguistici ricorrenti tramite K-Means.

L'ipotesi di partenza è che le descrizioni Luxury richiamino più frequentemente materiali, eleganza e raffinatezza, mentre quelle Fast Fashion privilegino praticità, quotidianità e accessibilità. La relazione tra prezzo e sentiment viene invece verificata senza presupporre che un prezzo più elevato corrisponda necessariamente a recensioni migliori.

## Dati e fonti

Il dataset analizzato contiene **100 prodotti**, suddivisi in:

- **59 Fast Fashion**
- **41 Luxury**

Per ciascun prodotto sono considerate le seguenti informazioni: `titolo_prodotto`, `categoria`, `prezzo`, `descrizione_brand` e `recensione_utente`. Durante l'elaborazione vengono generate ulteriori colonne, tra cui i testi puliti, i punteggi di sentiment e il cluster assegnato. Il file finale contiene **11 colonne**.

**Provenienza dei dati:** il dataset è stato **costruito a scopo dimostrativo all'interno del notebook**. Le descrizioni e le recensioni sono esempi creati per sperimentare il metodo; non provengono da un campionamento documentato di siti commerciali o piattaforme di recensioni. Per questo motivo l'analisi costituisce una **proof-of-concept** e i risultati non sono generalizzabili all'intero mercato.

### Strumenti e librerie

L'elaborazione è stata svolta su **Google Colab**, utilizzando il linguaggio Python e le librerie **pandas**, **NumPy**, **NLTK**, **scikit-learn**, **Matplotlib**, **Seaborn** e **WordCloud**. Per la sentiment analysis sono utilizzati un piccolo lessico italiano personalizzato e **VADER** (`SentimentIntensityAnalyzer`).

## Metodologia di analisi

### 1. Preparazione e pulizia dei testi

Le descrizioni e le recensioni vengono convertite in minuscolo, tokenizzate e filtrate per eliminare stopword italiane e numerosi elementi non alfanumerici. La pulizia rende i testi più uniformi e prepara i dati alle successive analisi lessicali.

### 2. Analisi lessicale con TF-IDF

Le descrizioni vengono trasformate in vettori numerici con `TfidfVectorizer`. Per l'esplorazione delle caratteristiche lessicali dei due segmenti viene utilizzato `max_features=10`, effettuando il fit separatamente sulle categorie. TF-IDF permette di considerare sia la frequenza dei termini sia la loro specificità rispetto ai documenti del corpus.

Una **WordCloud** per categoria completa l'esplorazione visiva delle parole ricorrenti; non costituisce una visualizzazione diretta dei pesi TF-IDF.

### 3. Sentiment analysis

Le recensioni sono analizzate attraverso due approcci:

- **Lessico italiano personalizzato:** assegna un punteggio in base alla presenza di parole considerate positive o negative.
- **VADER:** produce un punteggio sintetico di polarità (`compound`) per ciascuna recensione.

I punteggi vengono confrontati tra categorie e messi in relazione con il prezzo.

### 4. Clustering con K-Means

Le descrizioni pulite vengono nuovamente rappresentate con TF-IDF, utilizzando fino a **100 feature**, e raggruppate con **K-Means** impostando `n_clusters=2`, `random_state=42` e `n_init=10`.

L'assegnazione ai cluster avviene senza usare le categorie commerciali come etichette di addestramento. Una tabella di contingenza permette poi di confrontare i gruppi ottenuti con Fast Fashion e Luxury.

### 5. Visualizzazione e interpretazione

I risultati sono sintetizzati attraverso confronti dei prezzi, grafici del sentiment, scatter plot prezzo-sentiment, WordCloud e tabella dei cluster.

## Risultati principali

### Prezzo

| Indicatore | Fast Fashion | Luxury |
|---|---:|---:|
| Prezzo medio | €41,82 | €741,46 |
| Prezzo mediano | €29,99 | €610,00 |

Nel campione, il prezzo medio Luxury è circa **17,7 volte** quello Fast Fashion: la distinzione economica tra i segmenti risulta netta.

### Linguaggio delle descrizioni

Tra le feature lessicali individuate nell'esplorazione TF-IDF emergono:

- **Fast Fashion:** `casual`, `cotone`, `economica`, `economico`, `jeans`, `leggero`, `pantaloni`, `pratico`, `prezzo`, `stile`.
- **Luxury:** `borsa`, `eleganza`, `lusso`, `pelle`, `raffinato`, `sartoriale`, `seta`, `stile`, `taglio`, `tempo`.

Le descrizioni presentano quindi segnali lessicali coerenti con posizionamenti differenti, ma anche elementi comuni, come la parola **stile**.

### Sentiment e correlazione con il prezzo

Il sentiment medio delle recensioni calcolato con VADER è pari a circa:

- **Fast Fashion:** -0,031
- **Luxury:** -0,014

Entrambi i valori sono vicini allo zero. La correlazione tra **prezzo e sentiment VADER** è circa **r = 0,064**, mentre quella tra **prezzo e sentiment personalizzato** è circa **r = 0,045**.

Nel campione non emerge quindi una **relazione lineare apprezzabile** tra prezzo e sentiment. Il risultato non dimostra un rapporto causale e deve essere letto considerando i limiti degli strumenti utilizzati.

### Clustering

Il confronto tra cluster ottenuti e categorie originali restituisce:

| Cluster | Fast Fashion | Luxury |
|---|---:|---:|
| 0 | 59 | 26 |
| 1 | 0 | 15 |

Un sottogruppo di prodotti Luxury presenta caratteristiche lessicali sufficientemente distintive da essere raggruppato separatamente. Tuttavia, **26 prodotti Luxury condividono il cluster con tutti i prodotti Fast Fashion**: la separazione linguistica è quindi **parziale**.

## Limiti dell'analisi

Il progetto presenta alcuni limiti da considerare nell'interpretazione:

- **Dati costruiti:** il campione non è rappresentativo del mercato reale e può incorporare associazioni lessicali introdotte nella fase di creazione.
- **TF-IDF:** rappresenta soprattutto regolarità lessicali, senza cogliere pienamente il contesto semantico.
- **Sentiment personalizzato:** utilizza un lessico ridotto; nella versione attuale alcune espressioni composte da più parole non vengono riconosciute correttamente.
- **VADER:** è stato sviluppato principalmente per l'inglese e il suo utilizzo sulle recensioni italiane è esplorativo.
- **Clustering:** il numero di cluster (`k=2`) è stato scelto a priori e non validato attraverso il confronto con altri valori di k.

## Conclusioni

L'analisi evidenzia una **forte distinzione di prezzo**, alcune **differenze lessicali riconoscibili ma non assolute** e l'assenza di una **relazione lineare apprezzabile tra prezzo e sentiment** nel campione osservato.

Il progetto mostra come dati strutturati e testi possano essere analizzati insieme per esplorare il posizionamento commerciale, mantenendo una distinzione tra i risultati effettivamente osservati e le conclusioni che i dati consentono di sostenere.

## Possibili sviluppi

Per approfondire l'analisi sarebbe utile raccogliere un corpus reale e documentato di descrizioni e recensioni, ampliare il campione, adottare strumenti di sentiment validati per l'italiano, confrontare più valori di `k` con metriche di valutazione del clustering e sperimentare rappresentazioni semantiche più avanzate.

## Riproducibilità

Per riprodurre l'analisi, caricare il notebook `.ipynb` su **Google Colab** e aprirlo. Se necessario, installare le librerie principali in una cella del notebook:

```python
!pip install pandas numpy matplotlib seaborn nltk scikit-learn wordcloud
```

Eseguire le celle del notebook in ordine direttamente su Google Colab. Il notebook prepara le risorse NLTK necessarie, costruisce il dataset di esempio, svolge le analisi e salva il risultato elaborato in formato CSV.

**File principali del progetto:**

- Notebook Google Colab (`.ipynb`): codice e analisi.
- Dataset elaborato (`.csv`): dati e variabili derivate.
- Presentazione (`.pptx`): sintesi visiva di obiettivi, metodo e risultati.
- `README.md`: descrizione del progetto.

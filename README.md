# Olist E-commerce Analytics - Power BI Report

## 📊 Panoramica Progetto

Report di Business Intelligence sviluppato in Power BI per l'analisi delle performance dell'e-commerce Olist Store. Il progetto analizza ordini, ricavi e qualità del servizio nel periodo 2016-2018, fornendo insights strategici sulle dinamiche di crescita del business.

## 📁 Dataset Utilizzati

Il progetto si basa su 6 dataset principali dell'ecosistema Olist:

- `olist_orders_dataset` - Informazioni sugli ordini
- `olist_order_items_dataset` - Dettaglio prodotti per ordine
- `olist_products_dataset` - Catalogo prodotti
- `olist_order_reviews_dataset` - Recensioni e valutazioni clienti
- `olist_customers_dataset` - Anagrafica clienti
- `olist_category_name_translate_dataset` - Traduzione categorie prodotti

## 🔧 Data Modeling & Preparation

### Power Query
I dati sono stati processati e puliti utilizzando Power Query per garantire:
- Rimozione duplicati e valori nulli
- Normalizzazione formati date
- Standardizzazione campi testuali
- Creazione relazioni tra tabelle

### Data Model
Implementato un modello dimensionale con:
- Tabella calendario custom-built
- Relazioni one-to-many tra tabelle dimensionali e fatti
- Ottimizzazione delle cardinalità

## 📐 Misure DAX Implementate

### Tabella Calendario
```dax
Calendario = 
CALENDAR(
    MIN(Orders[purchase_timestamp]),
    MAX(Orders[purchase_timestamp])
)
```

Arricchita con colonne calcolate:
- `Month` - Mese numerico
- `Year` - Anno
- `Quarter` - Trimestre
- `MonthName` - Nome mese (formato testuale)
- `QuarterName` - Nome trimestre (formato Q1, Q2, etc.)

### KPI Principali

**Conteggio Ordini**
```dax
Conteggio Ordini = COUNTA('Order Items'[order_item_id])
```

**Numero Ordini Anno Precedente**
```dax
Numero Ordini PY = 
CALCULATE(
    [Conteggio Ordini], 
    PARALLELPERIOD(Calendario[Date], -12, MONTH)
)
```

**Ricavi Totali**
```dax
Ricavi Totali = SUM('Order Items'[Ricavo])
```

**Ricavi Anno Precedente**
```dax
Ricavi PY = 
CALCULATE(
    SUM('Order Items'[Ricavo]), 
    SAMEPERIODLASTYEAR(Calendario[Date])
)
```

**Variazione Percentuale Ordini (YoY)**
```dax
Variazione % Ordini = 
DIVIDE(
    [Conteggio Ordini] - [Numero Ordini PY],
    [Numero Ordini PY]
)
```

## 📈 Dashboard & Insights

Il report include visualizzazioni per analizzare:

- **Trend temporali**: Evoluzione ordini e ricavi con confronto year-over-year
- **Performance geografica**: Distribuzione vendite per stato/regione
- **Qualità servizio**: Analisi rating consegne e recensioni clienti
- **Categorie prodotto**: Performance per categoria merceologica

### Key Findings

- Crescita eccezionale 2016-2018 con tassi superiori al 20.000% YoY
- Forte concentrazione geografica nel mercato brasiliano
- Picchi stagionali significativi (novembre 2017: 8.665 ordini, 1,2M€ ricavi)
- Momentum positivo mantenuto nel 2018 nonostante basi comparative elevate

## 📝 Note

- I dati coprono il periodo settembre 2016 - ottobre 2018
- Alcune anomalie nei dati (es. crollo settembre 2018) richiedono ulteriore investigazione
- Il report è ottimizzato per analisi year-over-year

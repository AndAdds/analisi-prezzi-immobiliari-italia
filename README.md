# Prezzi immobiliari in Italia, 2016-2025

Analisi delle quotazioni immobiliari dell'Osservatorio del Mercato Immobiliare (OMI, Agenzia delle Entrate) su dieci anni. I prezzi sono confrontati per area geografica (Nord, Centro, Sud e Isole), per segmento (abitazioni, negozi, uffici) e per tipo di mercato (acquisto e affitto).

Progetto finale del corso Data Analytics & AI di Epicode. Strumenti: Python (pandas, matplotlib) e Power BI.

## Domande

1. Come sono cambiati i prezzi di acquisto e di affitto in dieci anni?
2. Ci sono differenze tra Nord, Centro e Sud e Isole?
3. In quali semestri gli aumenti sono stati più forti? Il rincaro percepito si vede nei dati?
4. In quali aree e in quale segmento il rendimento lordo (affitto annuo / prezzo) è più alto?

## Risultati

Abitazioni, valori nominali (non corretti per l'inflazione), variazione tra 2016-H1 e 2025-H2. Ogni area è riassunta con la mediana delle zone OMI.

| Area | Acquisto | Affitto |
|---|---|---|
| Nord | −2,3% | +8,0% |
| Centro | −12,5% | −1,2% |
| Sud e Isole | −7,8% | −2,0% |

- Nelle quotazioni OMI il rincaro percepito non emerge: l'acquisto delle abitazioni cala in tutte e tre le aree. L'aumento annuo più forte è +2,4% per l'acquisto (Nord, 2025-H1) e +3,8% per l'affitto (Centro, 2023-H2).
- Il risultato non dipende dal cambio delle zone nel tempo. Ripetendo il calcolo solo sulle zone presenti in entrambi i semestri, l'acquisto delle abitazioni varia di −10,3% al Centro, −5,4% al Sud e Isole e 0,0% al Nord.
- Per segmento, in Italia l'acquisto varia di −5,6% per le abitazioni, −11,6% per i negozi e −4,9% per gli uffici.
- Il rendimento lordo mediano nel 2025-H2 va dal 4,28% (abitazioni, Sud e Isole) al 6,53% (negozi, Nord).
- Il 17% dei prezzi di acquisto e il 22% degli affitti è identico dopo dieci anni: le quotazioni OMI cambiano a gradini e possono restare indietro rispetto al mercato.

## Dashboard

Dashboard Power BI a quattro pagine (panoramica, variazioni nel tempo, rendimento, acquisto e affitto a confronto).

- File: [dashboard/dashboard_prezzi_immobiliari.pbix](dashboard/dashboard_prezzi_immobiliari.pbix)
- Anteprima: [dashboard/dashboard_prezzi_immobiliari.pdf](dashboard/dashboard_prezzi_immobiliari.pdf)

La dashboard legge `data/processed/aggregato_per_powerbi.csv`.

## Contenuto del repository

```
notebooks/
  01_istat_ipab.ipynb      Indice dei prezzi delle abitazioni Istat: pulizia e controlli
  02_omi_pipeline.ipynb    Quotazioni OMI: dai 20 file grezzi alla tabella pulita
  03_analisi.ipynb         Analisi e tabella per la dashboard
data/processed/
  aggregato_per_powerbi.csv   Una riga per semestre, area e segmento
docs/
  criteri_fonti.md         Criteri di scelta delle fonti
  grafici/                 Grafici prodotti dal notebook 03
dashboard/                 File Power BI e anteprima PDF
```

## Come riprodurre

1. Scaricare i 20 file `QI_..._VALORI.csv` dal servizio *Forniture dati OMI* dell'Agenzia delle Entrate in `data/Raw/Omi/`.
2. Eseguire `notebooks/02_omi_pipeline.ipynb` (`Kernel > Restart Kernel and Run All Cells`, alcuni minuti). Produce `data/processed/omi_semestri_puliti.csv`.
3. Eseguire `notebooks/03_analisi.ipynb`. Produce i grafici e `aggregato_per_powerbi.csv`.
4. Aprire il file `.pbix` in Power BI Desktop e, se serve, aggiornare il percorso del CSV.

Servono Python 3 e le librerie pandas e matplotlib. I file grezzi OMI e il CSV pulito (centinaia di MB) non sono nel repository: i primi si scaricano dall'ente, il secondo si rigenera con il notebook 02.

## Dati e fonti

- **Quotazioni OMI**, Agenzia delle Entrate, Osservatorio del Mercato Immobiliare, 2016-2025, semestrali, licenza CC-BY 4.0. Circa 1,84 milioni di righe grezze, dopo la pulizia una riga per zona OMI, tipologia e semestre.
- **Indice dei prezzi delle abitazioni (IPAB)**, Istat, serie trimestrale. Trattato nel notebook 01.

Le scelte di pulizia (9 decisioni) e i problemi trovati nei dati sono documentati nel notebook 02.

## Limiti

- Le quotazioni OMI sono stime e non prezzi di vendita, e cambiano a gradini.
- Le aree sono tre: nascondono differenze tra regioni e tra città e campagna.
- I valori sono nominali. In termini reali il calo dei prezzi sarebbe maggiore.
- Il rendimento è lordo: non considera tasse, spese e periodi senza inquilino. Usa solo le righe con affitto su superficie lorda.
- Fusioni di comuni e il calo di zone nel 2024-H2 possono spostare le mediane. Il controllo sulle stesse zone (notebook 03, sezione 4.4) mostra che il risultato regge.
- L'analisi usa solo le quotazioni OMI: il confronto con l'indice Istat è un possibile sviluppo.

## Riferimenti

- Agenzia delle Entrate, OMI, *Nota metodologica: Compravendite immobiliari*, 21 maggio 2026 (aree geografiche OMI, descrizione delle quotazioni medie).
- Agenzia delle Entrate, OMI, *Nota metodologica: Locazioni immobiliari*, 3 giugno 2026 (canone mensile al m² nelle quotazioni).

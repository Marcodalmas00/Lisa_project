# Note progetto — Credit card fraud detection

Spazio per idee, letture e ragionamenti. Non sostituisce `docs/ASSIGNMENT_REQUIREMENTS.md`; va letto insieme ai requisiti ufficiali.

---

## 1) “Prediction superiore al 90%” qui non dice quasi nulla

Devi misurare quanto bene trovi i pochi “sì” (frodi).

La metrica giusta dipende da cosa vuoi evitare:

| Obiettivo | Metrica importante |
|-----------|-------------------|
| Voglio trovare più “sì” possibili | Recall / sensitivity |
| Voglio che quando dico “sì” sia quasi sempre vero | Precision |
| Voglio un equilibrio tra precision e recall | F1-score |
| Dataset molto sbilanciato | PR-AUC, più utile della ROC-AUC |
| Il falso negativo costa molto | Recall alto |
| Il falso positivo costa molto | Precision alta |

---

## 2) Non fare split casuale fatto male

Dividere train/test **mantenendo la proporzione delle classi** (stratificazione).

Esempio di ordine di grandezza:

- **Train:** ~80%  
- **Test:** ~20%  

Ordine di grandezza indicativo (numeri arrotondati):

- Train: ~200.000 casi, ~400 positivi  
- Test: ~50.000 casi, ~100 positivi  

*(Verificare sempre i conteggi reali sul CSV dopo lo split.)*

---

## 4) Class weights

Dire al modello che sbagliare un “sì” (o un “no”) pesa molto di più — allineare i pesi alle priorità business / costi FP vs FN.

---

## 5) Resampling — da tenere in considerazione

Possibilità per migliorare modelli base o anche avanzati: valutare se migliora qualcosa **con misure adeguate** (non solo accuracy).

Si può anche modificare il dataset di training. Tre direzioni:

**A. Undersampling dei “no”**  
Prendi tutti i positivi e solo una parte dei negativi.  
Esempio concettuale: 500 positivi, 5.000 negativi.

**B. Oversampling dei “sì”**  
Duplichi o aumenti artificialmente i positivi.  
Esempio: 500 positivi → 5.000 positivi tramite duplicazione o SMOTE.  
Attenzione: rischio overfitting, soprattutto se i positivi sono pochi.

**C. Combinazione (idea da testare)**  
- Tutti i positivi  
- Campione “intelligente” dei negativi  
- Class weights  
- Modello tipo gradient boosting  

Quindi: **non usare subito tutto il dataset in modo ingenuo** senza aver motivato split, metriche e bilanciamento.

---

## 6) Non usare la soglia 0.5 come default cieco

Il modello restituisce spesso una **probabilità**; in dataset sbilanciati `> 0.5` può essere sbagliatissimo.  
La soglia migliore può essere molto più bassa (es. 0.01, 0.03, 0.07) a seconda di quanti falsi positivi si tollerano.

Costruire una tabella tipo:

| Soglia | Precision | Recall | Falsi positivi (qualitativo) |
|--------|-----------|--------|------------------------------|
| 0.50 | alta es. ~90% | bassa es. ~5% | pochi |
| 0.10 | media es. ~60% | media es. ~35% | medi |
| 0.03 | più bassa es. ~35% | più alta es. ~75% | molti |
| 0.01 | bassa es. ~15% | molto alta es. ~90% | tantissimi |

*(I numeri sono solo esempio illustrativo — nel notebook vanno calcolati sui nostri modelli.)*

Domanda guida: **meglio perdere dei “sì” o accettare tanti falsi allarmi?** → collega a costi FP vs FN nel report.

---

## 8) Se i “sì” sono rarissimi: modello a due step (pipeline)

Idea operativa a due livelli:

1. **Step 1 — modello “largo”**  
   Molto sensibile: catturare quasi tutti i possibili “sì”.  
   Obiettivo: **recall molto alta**, anche a costo di molti falsi positivi.

2. **Step 2 — modello raffinato**  
   Lavora solo sui casi sospetti selezionati dallo step 1.  
   Obiettivo: **aumentare la precision** distinguendo meglio veri positivi da falsi allarmi.

Esempio di flusso concettuale:

```
~250.000 casi iniziali
  → Step 1 seleziona ~5.000 casi sospetti
  → Step 2 distingue meglio veri sì vs falsi allarmi
```

Strategia spesso più realistica che pretendere da **un solo** modello tutto perfetto.

---

## Collegamento al lavoro nel repo

- Metriche, split stratificato, class weights, threshold tuning e (opzionale) two-stage sono allineati a `docs/ASSIGNMENT_REQUIREMENTS.md`.
- Se implementiamo qualcosa da qui, nel notebook va **spiegato il perché** e **misurato l’effetto** (non solo l’idea).



"Il modello trova il 75–80% dei casi positivi, e tra i casi che segnala 1 su 3 / 1 su 2 è davvero positivo."
# Detekcia škodlivých URL pomocou metód dátovej analýzy

Tento repozitár obsahuje praktickú časť bakalárskej práce zameranú na klasifikáciu URL adries pomocou metód dátovej analýzy. Projekt pracuje s datasetom: https://www.kaggle.com/datasets/sid321axn/malicious-urls-dataset, z ktorého sa extrahujú príznaky URL adries a následne sa trénuje a vyhodnocuje 5 klasifikačných modelov.

# Cieľ projektu

Cieľom projektu je porovnať viaceré klasifikačné modely pri rozpoznávaní bezpečných a škodlivých URL adries.

Projekt rieši dve klasifikačné úlohy:

* binárna klasifikácia - rozdelenie URL na: `benign` a `malicious`,
* multitriedna klasifikácia - rozdelenie URL podľa pôvodných tried datasetu: `benign`, `defacement`, `phishing` a `malware`.

# Použité technológie

Projekt je vytvorený v jazyku Python a spracovaný vo forme Jupyter Notebooku.

Použité knižnice:

* `pandas`
* `numpy`
* `matplotlib`
* `scikit-learn`
* `xgboost`
* `imbalanced-learn`

# Štruktúra projektu

```text
.
├── bakalarka.ipynb
├── malicious_phish.csv
├── bakalarka_outputs/
└── README.md
```

Po spustení notebooku sa automaticky vytvorí priečinok `bakalarka_outputs`, ktorý obsahuje výsledky experimentov, tabuľky a grafy.

# Dataset

Projekt očakáva vstupný súbor z https://www.kaggle.com/datasets/sid321axn/malicious-urls-dataset vo forme:

```text
malicious_phish.csv
```

Dataset musí byť uložený v rovnakom priečinku ako notebook.

# Extrakcia príznakov

Z každej URL adresy sa extrahuje viacero príznakov, napríklad:

* dĺžka URL, domény, cesty a query časti,
* počet bodiek, lomiek, číslic, špeciálnych znakov a parametrov,
* prítomnosť HTTPS,
* výskyt IP adresy namiesto domény,
* počet subdomén,
* výskyt skracovačov URL,
* výskyt podozrivých slov ako `login`, `secure`, `verify`, `bank` alebo `password`,
* štatistiky tokenov v URL,
* pomer číslic, písmen, veľkých písmen a špeciálnych znakov.

Notebook následne vytvára aj analýzu príznakov, napríklad štatistické ukazovatele, početnosti binárnych príznakov a korelačnú maticu.

# Výber príznakov

Pred trénovaním modelov sa vykonáva výber najdôležitejších príznakov:

1. odstránenie silno korelovaných príznakov podľa nastavenej hranice,
2. výber `TOP_K` najlepších príznakov pomocou metódy:

   * `mutual_info_classif`, alebo
   * `chi2`.

Predvolené nastavenia:

```python
RANDOM_STATE = 42
TEST_SIZE = 0.20
USE_OVERSAMPLING = False
USE_MI = True
CORRELATION_THRESHOLD = 0.90
TOP_K = 20
```

# Použité modely

V projekte sa porovnávajú tieto modely:

* Logistic Regression
* Decision Tree
* Random Forest
* LinearSVC
* XGBoost

# Vyhodnotenie modelov

Modely sú vyhodnocované pomocou metrík:

* Accuracy
* Precision
* Recall
* F1-score
* Macro F1-score
* ROC AUC *(pri binárnej klasifikácii)*

Notebook zároveň ukladá:

* tabuľky výsledkov vo formáte CSV,
* matice zámen,
* ROC krivky,
* porovnanie modelov podľa Macro F1-score,
* zoznam vybraných príznakov,
* skóre príznakov.

# Výstupy

Výstupy sa ukladajú do priečinka:

```text
bakalarka_outputs/
```

Príklady generovaných výstupov:

```text
bakalarka_outputs/
├── feature_analysis/
│   ├── numeric_feature_statistics.csv
│   ├── binary_feature_frequencies.csv
│   ├── full_correlation_matrix.csv
│   ├── high_correlation_pairs.csv
│   └── full_correlation_heatmap.png
├── binary/
│   ├── binary_results.csv
│   ├── binary_feature_scores.csv
│   ├── binary_selected_features.txt
│   └── roc_curves/
├── multiclass/
│   ├── multiclass_results.csv
│   ├── multiclass_feature_scores.csv
│   └── multiclass_selected_features.txt
└── all_results_summary.csv
```

# Inštalácia

Odporúča sa vytvoriť virtuálne prostredie:

```bash
python -m venv .venv
```

Aktivácia prostredia vo Windows:

```bash
.venv\Scripts\activate
```

Aktivácia prostredia v Linuxe alebo macOS:

```bash
source .venv/bin/activate
```

Inštalácia potrebných knižníc:

```bash
pip install pandas numpy matplotlib scikit-learn xgboost imbalanced-learn jupyter
```

# Spustenie projektu

1. Uložte dataset `malicious_phish.csv` do rovnakého priečinka ako notebook.
2. Spustite Jupyter Notebook:

```bash
jupyter notebook
```

3. Otvorte súbor:

```text
bakalarka.ipynb
```

4. Spustite bunky postupne od začiatku.

Po úspešnom spustení sa vygeneruje priečinok `bakalarka_outputs` so všetkými výsledkami.

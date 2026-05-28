# Detekcia skodlivych URL pomocou metod datovej analyzy

Tento repozitar obsahuje prakticku cast bakalarskej prace zameranu na klasifikaciu URL adries pomocou metod datovej analyzy. Projekt pracuje s datasetom: https://www.kaggle.com/datasets/sid321axn/malicious-urls-dataset, z ktoreho sa extrahuju priznaky URL adries a nasledne sa trenuje a vyhodnocuje 5 klasifikacnych modelov.

# Ciel projektu

Cielom projektu je porovnat viacere klasifikacne modely pri rozpoznavani bezpecnych a skodlivych URL adries.

Projekt riesi dve klasifikacne ulohy:

- binarna klasifikacia - rozdelenie URL na: `benign` a `malicious`,
- multitriedna klasifikacia - rozdelenie URL podla povodnych tried datasetu: `benign`, `defacement`, `phishing` a `malware`.

# Pouzite technologie

Projekt je vytvoreny v jazyku Python a spracovany vo forme Jupyter Notebooku.

Pouzite kniznice:

- `pandas`
- `numpy`
- `matplotlib`
- `scikit-learn`
- `xgboost`
- `imbalanced-learn` 

# Struktura projektu

```text
.
├── bakalarka.ipynb
├── malicious_phish.csv
├── bakalarka_outputs/
└── README.md
```

Po spusteni notebooku sa automaticky vytvori priecinok `bakalarka_outputs`, ktory obsahuje vysledky experimentov, tabulky a grafy.

# Dataset

Projekt ocakava vstupny subor z https://www.kaggle.com/datasets/sid321axn/malicious-urls-dataset vo forme:

```text
malicious_phish.csv
```

Dataset musi byt ulozeny v rovnakom priecinku ako notebook.

# Extrakcia priznakov

Z kazdej URL adresy sa extrahuje viacero priznakov, napriklad:

- dlzka URL, domeny, cesty a query casti,
- pocet bodiek, lomiek, cislic, specialnych znakov a parametrov,
- pritomnost HTTPS,
- vyskyt IP adresy namiesto domeny,
- pocet subdomen,
- vyskyt skracovacov URL,
- vyskyt podozrivych slov ako `login`, `secure`, `verify`, `bank` alebo `password`,
- statistiky tokenov v URL,
- pomer cislic, pismen, velkych pismen a specialnych znakov.

Notebook nasledne vytvara aj analyzu priznakov, napriklad statisticke ukazovatele, pocetnosti binarnych priznakov a korelacnu maticu.

# Vyber priznakov

Pred trenovanim modelov sa vykonava vyber najdolezitejsich priznakov:

1. odstranenie silno korelovanych priznakov podla nastavenej hranice,
2. vyber `TOP_K` najlepsich priznakov pomocou metody:
   - `mutual_info_classif`, alebo
   - `chi2`.

Predvolene nastavenia:

```python
RANDOM_STATE = 42
TEST_SIZE = 0.20
USE_OVERSAMPLING = False
USE_MI = True
CORRELATION_THRESHOLD = 0.90
TOP_K = 20
```

# Pouzite modely

V projekte sa porovnavaju tieto modely:

- Logistic Regression
- Decision Tree
- Random Forest
- LinearSVC
- XGBoost

# Vyhodnotenie modelov

Modely su vyhodnocovane pomocou metrik:

- Accuracy
- Precision
- Recall
- F1-score
- Macro F1-score
- ROC AUC *(pri binárnej klasifikácii)*

Notebook zaroven uklada:

- tabulky vysledkov vo formate CSV,
- matice zamen,
- ROC krivky,
- porovnanie modelov podla Macro F1-score,
- zoznam vybranych priznakov,
- skore priznakov.

# Vystupy

Vystupy sa ukladaju do priecinka:

```text
bakalarka_outputs/
```

Priklady generovanych vystupov:

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

# Instalacia

Odporuca sa vytvorit virtualne prostredie:

```bash
python -m venv .venv
```

Aktivacia prostredia vo Windows:

```bash
.venv\Scripts\activate
```

Aktivacia prostredia v Linuxe alebo macOS:

```bash
source .venv/bin/activate
```

Inatalacia potrebnych kniznic:

```bash
pip install pandas numpy matplotlib scikit-learn xgboost imbalanced-learn jupyter
```

# Spustenie projektu

1. Ulozte dataset `malicious_phish.csv` do rovnakeho priecinka ako notebook.
2. Spustite Jupyter Notebook:

```bash
jupyter notebook
```

3. Otvorte subor:

```text
bakalarka.ipynb
```

4. Spustite bunky postupne od zaciatku.

Po uspesnom spusteni sa vygeneruje priecinok `bakalarka_outputs` so vsetkymi vysledkami.


# Bangla Abusive Regional Dialect Dataset

A balanced Bengali-script dataset for **regional dialect classification of abusive and hostile text** across four Bangla language varieties:

- Standard Bangla
- Chattagram
- Sylhet
- Barishal

The dataset is designed for research on **Bangla regional dialect identification**, particularly in abusive or hostile-language contexts.

## Dataset Overview

| Property | Value |
|---|---:|
| Total samples | 1,000 |
| Number of classes | 4 |
| Samples per class | 250 |
| Text language | Bangla |
| Script | Bengali |
| Task | Multi-class dialect classification |
| Input | Abusive/hostile text |
| Target | Regional dialect label |
| Missing values | 0 |
| Duplicate rows | 0 |

### Class Distribution

| Dialect label | Samples |
|---|---:|
| Standard Bangla | 250 |
| Chattagram | 250 |
| Sylhet | 250 |
| Barishal | 250 |
| **Total** | **1,000** |

## Dataset Format

The dataset is provided as an Excel file:

`bangla_abusive_dialect_text.xlsx`

It contains exactly two columns:

| Column | Description |
|---|---|
| `text` | Abusive or hostile Bangla sentence |
| `dialect_label` | Regional variety of the sentence |

### Example

| text | dialect_label |
|---|---|
| তুই একদম বোকা। | Standard Bangla |
| তুঁই এক্কেরে বেকুব। | Chattagram |
| তুমি এক্কেরে বেকুব। | Sylhet |
| তুই এক্কেবারে বেকুব। | Barishal |

## Research Task

The primary task is:

> **Given an abusive or hostile Bangla sentence, identify its regional dialect.**

Formally,

```text
Input:  Bangla abusive/hostile sentence

Output:
    Standard Bangla
    Chattagram
    Sylhet
    Barishal
```

The dataset is **not intended for binary abusive-language detection**, because all samples belong to the abusive/hostile domain. The classification target is only the regional dialect.

## Dataset Construction

The dataset was developed as a **corpus-grounded synthetic regional-language dataset**.

Regional sentence construction was guided by recurring linguistic patterns observed in existing Bangla regional-language data, including variation in:

- pronouns
- possessive forms
- verb morphology
- negation patterns
- interrogative forms
- postpositions
- regional vocabulary
- Bengali-script spelling conventions

The goal was to preserve the abusive or hostile meaning while expressing it through regionally characteristic linguistic forms.

## Important Validation Note

This dataset should currently be treated as a **research/prototyping dataset rather than a native-speaker gold-standard corpus**.

Although the regional sentences were constructed using patterns observed in regional-language datasets, synthetic dialect generation can still introduce:

- unnatural expressions
- mixed-dialect constructions
- inconsistent spelling
- overuse of highly recognizable regional markers
- differences in how native speakers from different subregions would naturally phrase the same utterance

Before using the dataset as a formally released linguistic benchmark, native speakers from **Chattagram, Sylhet, and Barishal** should review and, where necessary, revise the regional sentences.

## Recommended Native-Speaker Validation

For a publication-quality release, we recommend:

1. At least two native speakers for each regional variety.
2. Independent review of every regional sentence.
3. Evaluation of:
   - dialect naturalness
   - grammatical acceptability
   - semantic preservation
   - abusive-intensity preservation
   - spelling/orthographic acceptability
4. Adjudication when reviewers disagree.
5. Reporting inter-annotator agreement in the associated paper.

## Recommended Experimental Protocol

A basic dialect-classification experiment can use models such as:

- BanglaBERT
- BanglaBERT Base
- mBERT
- XLM-RoBERTa
- character-level CNN/LSTM models
- classical TF-IDF + SVM baselines

Because regional variation is often strongly represented through spelling and morphology, **character n-gram baselines** are also recommended.

### Evaluation Metrics

Recommended metrics include:

- Accuracy
- Macro Precision
- Macro Recall
- Macro F1-score
- Weighted F1-score
- Matthews Correlation Coefficient (MCC)
- Confusion Matrix

Macro F1-score is especially useful for reporting balanced multi-class performance.

## Python Usage

```python
import pandas as pd

df = pd.read_excel("bangla_abusive_dialect_text.xlsx")

print(df.head())
print(df.shape)
print(df["dialect_label"].value_counts())
```

### Preparing Features and Labels

```python
X = df["text"]
y = df["dialect_label"]
```

### Example Label Encoding

```python
from sklearn.preprocessing import LabelEncoder

encoder = LabelEncoder()
y_encoded = encoder.fit_transform(y)

print(dict(zip(encoder.classes_, encoder.transform(encoder.classes_))))
```

## Recommended Train/Test Splitting

For an initial experiment:

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    df["text"],
    df["dialect_label"],
    test_size=0.20,
    random_state=42,
    stratify=df["dialect_label"]
)
```

For the final research benchmark, semantic similarity and near-duplicate leakage should be checked before splitting. If parallel or semantically matched versions are retained in future releases, all versions of the same semantic item should remain in the **same partition**.

## Potential Research Directions

This dataset can support research on:

- Bangla regional dialect identification
- dialect-aware abusive-language NLP
- low-resource regional language modeling
- cross-dialect robustness of Bangla language models
- character-level versus token-level dialect recognition
- regional lexical and morphological variation
- transfer learning across Bangla varieties
- explainable dialect classification

## Ethical Considerations

The dataset contains abusive and hostile language and may include text that some users find offensive.

It should be used primarily for legitimate research purposes such as:

- language technology
- dialect identification
- moderation research
- robustness evaluation
- low-resource NLP

Researchers should avoid using the dataset to target, stereotype, or discriminate against speakers of any regional variety.

A dialect label describes a **linguistic variety in the dataset** and should not be treated as evidence about the identity, behavior, or characteristics of individual speakers.

## Limitations

Current limitations include:

- relatively small dataset size
- synthetic construction
- no completed native-speaker validation
- possible intra-regional variation not fully represented
- possible orthographic inconsistency
- abusive-language domain only
- Bengali-script text only
- no Romanized Bangla
- no audio or phonological information

These limitations should be considered when interpreting experimental results.

## Repository Structure

A minimal GitHub repository can use the following structure:

```text
bangla-abusive-regional-dialect-dataset/
│
├── README.md
├── data/
│   └── bangla_abusive_dialect_text.xlsx
└── LICENSE
```

Do not add a license until you have verified that your intended license is compatible with the licenses or usage terms of any source datasets used during dataset construction.

## Citation

If you use this dataset in research, please cite the associated paper or dataset release.

Until a formal publication is available, you may use a temporary citation such as:

```bibtex
@dataset{bangla_abusive_regional_dialect_dataset,
  title        = {Bangla Abusive Regional Dialect Dataset},
  year         = {2026},
  note         = {Dataset for four-class regional dialect classification of abusive Bangla text}
}
```

Replace this temporary entry with the official paper, DOI, authors, repository URL, and release information once available.

## Version

**Version 1.0 — 2026**

Initial research release containing 1,000 balanced samples across four regional dialect classes.

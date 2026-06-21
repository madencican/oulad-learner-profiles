# Beyond a Single Outcome: Behavioral Learner Profiles in OULAD

Reproducibility repository for the CEIT 720 (Learning Analytics in Education)
research paper:

> **Beyond a Single Outcome: Evaluating Behavioral Learner Profiles in OULAD
> Against Academic Success, Engagement, and Course Completion**
> Batuhan C. Madenci — Intelligent Systems Engineering, Graduate Education Institute,
> Bursa Technical University.

## Overview

This project applies **K-medoids (PAM)** clustering to four behavioral features from the
**Open University Learning Analytics Dataset (OULAD)** and evaluates the resulting
learner profiles against **three distinct outcomes**—academic success, engagement, and
course completion/withdrawal—rather than a single performance measure.

Four robust, well-separated profiles emerge (*disengaged non-starters*, *struggling
partial participants*, *consistent completers*, *highly engaged achievers*). They
discriminate final course outcome—a variable never used in clustering
(χ² test, Cramér's V = 0.51)—and separate **withdrawal from failure** as distinct risk
pathways.

## Research questions

- **RQ1** — What distinct behavioral profiles emerge from VLE interaction and assessment data?
- **RQ2** — Do the profiles differ in academic success (assessment scores)?
- **RQ3** — Do the profiles differ in engagement (VLE clicks, active days)?
- **RQ4** — Do the profiles differ in course completion and withdrawal?

## Methods

Two distinct learning-analytics methods (per course requirement):

1. **K-medoids / PAM clustering** (FasterPAM), with **Average Silhouette Width (ASW)**
   for selecting *k*; final solution at **k = 4**.
2. **Statistical group comparisons** — Kruskal–Wallis H test (with ε² effect size and
   Dunn/Bonferroni post-hoc) for continuous outcomes, and the χ² test of independence
   (with Cramér's V) for the categorical course outcome.

## Repository contents

```
.
├── README.md
├── notebook.ipynb        # full, end-to-end analysis pipeline
├── paper.pdf                       # the compiled paper
├── asw_vs_k.png                    # Figure 1 (ASW vs k)
├── fig1_outcome_composition.png    # Figure 3 (outcome composition)
└── fig2_fingerprint.png            # Figure 2 (behavioral fingerprint)
```

## Data access (the dataset is NOT included here)

OULAD is released under **CC-BY 4.0** by Kuzilek, Hlosta, & Zdrahal (2017). The raw
data is **not redistributed** in this repository; the notebook downloads it
automatically at runtime from the stable UCI mirror:

- Official site: <https://analyse.kmi.open.ac.uk/open_dataset>
- UCI mirror (used by the notebook): <https://archive.ics.uci.edu/dataset/349/open+university+learning+analytics+dataset>

No manual download is required—running the notebook fetches and unpacks the four
tables used (`studentInfo`, `studentVle`, `studentAssessment`, `assessments`).

## How to reproduce

1. Open `notebook.ipynb` in **Google Colab** (or any Jupyter environment).
2. Run all cells top to bottom. The first cells install dependencies and download OULAD.
3. Outputs (cluster profiles, statistical tests, and the three figures) regenerate end to end.

### Dependencies

```bash
pip install kmedoids scikit-posthocs pandas numpy scipy scikit-learn matplotlib seaborn
```

A fixed random seed (`RANDOM_STATE = 42`) is used throughout for reproducibility.

## Key results

| Profile | n | Pass+Dist. | Withdrawn |
|---|---:|---:|---:|
| Disengaged non-starters | 7,302 | 0.1% | 80.0% |
| Struggling partial participants | 6,779 | 2.6% | 52.0% |
| Consistent completers | 11,802 | 76.7% | 5.6% |
| Highly engaged achievers | 6,710 | 91.7% | 1.9% |

χ²(9) = 25,277.5, p < .001, Cramér's V = 0.51.

## AI tool disclosure

The AI assistant **Claude (Anthropic)** was used for translating the analysis pipeline
from R to Python, for debugging, and for drafting/editing assistance. All analytical
decisions, code, and interpretations were reviewed by and are the responsibility of the
author, in line with the course policy on disclosure of AI tools.

## License

Code in this repository: MIT. The OULAD dataset is licensed separately under CC-BY 4.0
by its original authors.

## References

- Kuzilek, J., Hlosta, M., & Zdrahal, Z. (2017). Open University Learning Analytics
  dataset. *Scientific Data*, 4, 170171.
- Murphy, K., López-Pernas, S., & Saqr, M. (2024). Dissimilarity-based cluster analysis
  of educational data. In *Learning Analytics Methods and Tutorials*. Springer.
- Schubert, E., & Rousseeuw, P. J. (2021). Fast and eager k-medoids clustering.
  *Information Systems*, 101, 101804.

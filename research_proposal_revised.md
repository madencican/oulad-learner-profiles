# Research Idea: CEIT 720 Learning Analytics in Education

---

## 1. Research Problem

Educational trace data — clickstreams, assessment logs, and interaction records — has grown substantially in online learning environments, yet its abundance does not automatically translate into insight. A critical challenge is identifying which behavioral patterns are educationally meaningful and for whom. Clustering offers a principled, data-driven way to address this: by grouping students with similar behavioral profiles, it makes hidden structure in the data visible and interpretable.

Clustering has been widely applied to OULAD and similar datasets to identify learner profiles, but the resulting profiles are typically evaluated against a single outcome variable — most often academic performance. Studies clustering OULAD students by VLE interaction patterns have consistently linked the resulting profiles to final grades or pass/fail status alone, without separately testing completion or withdrawal as a distinct outcome (Lima et al., 2023; Zhang et al., 2023). Where engagement is considered, it is typically used as an *input* to the clustering process itself — defining the behavioral features that produce the clusters — rather than as a separate outcome tested against cluster membership after the fact (Lima et al., 2023; Alzahrani et al., 2025). Even Alzahrani et al. (2025), who explicitly note that prior research "predominantly emphasize[s] final outcomes such as course completion or final grades," ultimately validate their own engagement clusters against a single composite performance score. To our knowledge, no published OULAD-based clustering study has systematically tested the same set of learner profiles against academic success, engagement, and completion/withdrawal as three separately evaluated outcomes. This narrow evaluation limits the practical utility of identified profiles, since dropout risk and engagement trajectories carry independent instructional relevance from academic performance.

This study addresses that gap by applying clustering to behavioral data from the Open University Learning Analytics Dataset (OULAD) and evaluating the resulting learner profiles across three distinct outcome dimensions: academic success, engagement, and course completion. The contribution is not a new algorithm, but a more complete evaluation framework — one that asks whether the same cluster structure is meaningful across multiple educationally relevant outcomes simultaneously.

---

## 2. Research Questions

**RQ1:** What distinct learner behavioral profiles emerge from students' VLE interaction and assessment data?

**RQ2:** Do the identified profiles differ significantly in terms of academic success (final assessment scores)?

**RQ3:** Are the identified profiles associated with differences in engagement levels (VLE interaction frequency and active days)?

**RQ4:** Do the identified profiles differ in course completion and withdrawal rates?

---

## 3. Research Data

This study uses the Open University Learning Analytics Dataset (OULAD) (Kuzilek et al., 2017), a publicly available, anonymized dataset containing records from over 32,000 students enrolled in undergraduate modules at The Open University. Four tables are used:

- **studentVle** — daily VLE interaction logs (clicks per activity, active days)
- **studentAssessment** — individual assignment submission records and scores
- **assessments** — assessment metadata (type, weight, and the module-presentation each assessment belongs to), used to compute the submission-rate denominator per module-presentation and to exclude final exams, whose scores are largely missing in the dataset
- **studentInfo** — final course outcome (Pass, Fail, Withdrawn, Distinction) and demographic information

From these tables, four behavioral features are derived: total VLE clicks, number of active days, assessment submission rate, and mean assessment score.

---

## 4. LA Methods

### 4.1 Clustering (RQ1)

Following the dissimilarity-based cluster analysis framework of Chapter 8 (Murphy, López-Pernas, & Saqr, 2024), the four behavioral features are standardized using Z-score normalization prior to clustering, ensuring that no single variable dominates the distance calculation due to scale differences.

K-medoids (PAM) is selected as the clustering algorithm. Unlike K-means, which uses the arithmetic mean as the cluster center, K-medoids selects an actual data point (the medoid) as the representative of each cluster. This makes it substantially more robust to extreme behavioral patterns — a relevant concern in VLE log data, where a small subset of students may exhibit unusually high or low activity levels. The optimal number of clusters is determined by the Average Silhouette Width (ASW), which measures how well each observation fits its assigned cluster relative to neighboring clusters. ASW is algorithm-agnostic and does not assume any particular distributional structure, making it appropriate for dissimilarity-based methods.

### 4.2 Statistical Group Comparisons (RQ2–RQ4)

Once cluster labels are assigned, they are merged with the studentInfo and studentVle tables for outcome evaluation. Each research question is addressed with a distinct statistical test matched to the nature of the outcome variable:

| Research Question | Outcome Variable | Statistical Test | Rationale |
|---|---|---|---|
| RQ2 — Academic success | Mean assessment score (continuous) | Kruskal-Wallis H test | Non-parametric; appropriate for non-normal score distributions |
| RQ3 — Engagement | Total VLE clicks, active days (continuous) | Kruskal-Wallis H test | Same rationale; engagement metrics are typically right-skewed |
| RQ4 — Completion | Pass / Fail / Withdrawn / Distinction (categorical) | Chi-square test of independence | Tests association between cluster membership and outcome category |

Where Kruskal-Wallis yields a significant result, post-hoc pairwise comparisons (Dunn test with Bonferroni correction) will be conducted to identify which specific cluster pairs differ.

All analyses are implemented in Python and will be made available as a reproducible Jupyter Notebook alongside the manuscript.

---

## References

Kuzilek, J., Hlosta, M., & Zdrahal, Z. (2017). Open University Learning Analytics dataset. *Scientific Data, 4*, 170171. https://doi.org/10.1038/sdata.2017.171

Murphy, K., López-Pernas, S., & Saqr, M. (2024). Dissimilarity-based cluster analysis of educational data: A comparative tutorial using R. In M. Saqr & S. López-Pernas (Eds.), *Learning Analytics Methods and Tutorials*. Springer. https://doi.org/10.1007/978-3-031-54464-4_8

Saqr, M., & López-Pernas, S. (Eds.). (2024). *Learning Analytics Methods and Tutorials*. Springer. https://doi.org/10.1007/978-3-031-54464-4

Lima, G. D. O., Costa, J. A. R., Araújo, R. D., & Dorça, F. A. (2023). Exploring the relationship between students' engagement and self-regulated learning: A case study using OULAD dataset and machine learning techniques. In *Anais do XXXIV Simpósio Brasileiro de Informática na Educação (SBIE)* (pp. 1154–1165). https://doi.org/10.5753/sbie.2023.234344

Zhang, J., Qiu, F., Wu, W., Wang, J., Li, R., Guan, M., & Huang, J. (2023). E-learning behavior categories and influencing factors of STEM courses: A case study of the Open University Learning Analysis Dataset (OULAD). *Sustainability, 15*(10), 8235. https://doi.org/10.3390/su15108235

Alzahrani, N., Meccawy, M., Samra, H., & El-Sabagh, H. A. (2025). Identifying weekly student engagement patterns in e-learning via K-means clustering and label-based validation. *Electronics, 14*(15), 3018. https://doi.org/10.3390/electronics14153018

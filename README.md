# CEO Departure Narratives: Crisis vs. Normal Period Text Analysis

A text mining analysis examining how CEO departure announcements differ between the 2008–2009 financial crisis and stable economic periods. This project applies multiple unsupervised learning methods to discover linguistic and thematic patterns in corporate communication during economic stress.

## Research Question

**Do CEO departure narratives fundamentally change during economic crises compared to normal times?**

Specifically, this analysis investigates whether CEO departures from S&P 1500 firms are described differently during the 2008–2009 financial crisis compared to the surrounding stable periods (2003–2007, 2010–2014).

## Key Findings

### Vocabulary Differences (Keyness Analysis)
- **Crisis-period departures** explicitly referenced external economic conditions ("recession") to explain problems, potentially externalizing blame to macroeconomic forces
- **Normal-period departures** used language emphasizing individual agency ("personal," "elected") to frame exits as internal choices
- Crisis notes contained more company-specific names (Wilmington, Motorola, Exxon), reflecting industry-concentrated stress during 2008–2009

### Thematic Differences (BERTopic)
- All 11 discovered topics appeared in both periods, but with shifted prevalence
- Crisis-heavy topics involved external pressure, future leadership transitions, and voluntary step-downs
- Normal-heavy topics featured CEO-to-chairman transitions and governance changes
- ~40% of documents classified as outliers, reflecting the formulaic nature of corporate announcements

### Structural Differences (K-Medoids Clustering)
- Crisis narratives split into **4 uneven clusters** with varying detail levels
- Normal narratives split into **2 balanced clusters** differing primarily in tone
- When clustering all departures together, crisis and normal documents mixed proportionally across clusters rather than separating—indicating similar underlying templates regardless of economic period

### Overall Conclusion
Crisis doesn't create entirely different narrative structures, but it does shape communication in three ways:
1. **Vocabulary choice**: External economic attribution vs. internal/personal framing
2. **Thematic emphasis**: More focus on pressure and abrupt changes
3. **Strategic diversity**: More varied approaches as companies navigate difficult circumstances

## Methodology

### Data
- **Source**: [Tidy Tuesday CEO Departures Dataset](https://github.com/rfordatascience/tidytuesday/tree/master/data/2021/2021-04-27) (2021, Week 18)
- **Scope**: 9,423 CEO departure events from S&P 1500 firms (1987–2020)
- **Analysis subset**: 3,547 departures from 2003–2014 (544 crisis, 3,003 normal)

### Text Preprocessing
- Lemmatization using UDPipe (English EWT model)
- Unigram + bigram tokenization
- Minimal stopword removal (preserving most words for topic modeling)
- Feature trimming: terms appearing in ≥2 documents

### Analytical Methods

| Method | Purpose | Implementation |
|--------|---------|----------------|
| **LDA** (Latent Dirichlet Allocation) | Discover latent topics across all departures | K=6 topics via Gibbs sampling (3000 iterations) |
| **Keyness Analysis** | Identify words that statistically distinguish crisis from normal periods | Chi-squared test via `quanteda` |
| **BERTopic** | Semantic topic discovery using document embeddings | BAAI/bge-m3 embeddings + HDBSCAN clustering |
| **K-Medoids Clustering** | Test whether vocabulary differences create distinct narrative clusters | PAM algorithm on TF-IDF weighted DFM |
| **Isolation Forest** | Anomaly detection to identify unusual departure narratives | 5% contamination threshold |

### Why Unsupervised Learning?
A supervised classifier could predict crisis vs. normal, but would only confirm the periods are different. The research question is exploratory: discovering *what patterns exist* and *how they relate to economic context*. Unsupervised methods allow the data to reveal structure without imposing assumptions.

## Repository Structure

```
├── README.md
├── analysis.qmd          # Quarto source with code and narrative
├── analysis.html         # Rendered report (viewable in browser)
└── data/                 # Data directory (see Data Access below)
```

## Reproducing the Analysis

1. Clone this repository
2. Install R packages (the script uses `pacman::p_load()` for automatic installation)
3. Set up Python environment for BERTopic (update the path in the `bertopic-setup` chunk)
4. Render the Quarto document:
   ```bash
   quarto render analysis.qmd
   ```

**Note on BERTopic reproducibility**: Results include fixed random seeds for UMAP (`random_state = 33L`) and LDA (`seed = 42`), but minor variations may occur across different hardware/software configurations.

## Limitations

- **Crisis period definition**: Sharp 2008–2009 boundaries may not capture gradual economic stress
- **Sample imbalance**: 5× more normal-period departures than crisis-period
- **Mixed text sources**: Dataset contains both original announcements and researcher summaries
- **Industry concentration**: Financial sector overrepresented in crisis sample
- **Limited close reading**: Interpretations based on 3–5 examples per topic/cluster

## Ethical Considerations

- Analysis involves real executives whose decisions were made under extraordinary circumstances
- Findings about communication patterns could potentially be misused to craft misleading announcements
- Institutional investors benefit more from such analysis than employees affected by departures

---

*Analysis completed as part of DS202A coursework at the London School of Economics and Political Science

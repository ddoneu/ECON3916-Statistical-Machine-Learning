# FedSpeak Analysis — NLP on FOMC Minutes

## Objective

Quantify shifts in Federal Reserve communication over two decades by applying NLP techniques to FOMC meeting minutes, transforming unstructured policy text into measurable sentiment and thematic signals.

## Methodology

- Collected 240 FOMC meeting minutes (2000-2026) via the HuggingFace `vtasca/fomc-statements-minutes` dataset
- Preprocessed text with NLTK: lowercasing, non-alphabetic removal, tokenization, stop word filtering, and WordNet lemmatization
- Built a TF-IDF document-term matrix (5,000 features, unigrams and bigrams) with min_df=5 and max_df=0.85 to filter noise and background terms
- Scored each document using the Loughran-McDonald financial sentiment lexicon, computing net sentiment and an uncertainty index
- Visualized rolling sentiment and uncertainty trends with event annotations (Lehman collapse, taper tantrum, COVID, 2022 tightening cycle)
- Reduced TF-IDF dimensionality to 50 components via TruncatedSVD, then applied K-Means (K=3) to identify latent language regimes
- Compared pre-COVID vs. post-COVID sentiment distributions using overlaid histograms

## Key Findings

- Net sentiment averaged slightly positive (0.007) across the full sample, with December 2008 as the most negative meeting and June 2004 as the most positive, aligning with the financial crisis trough and mid-2000s expansion respectively
- K-Means clustering identified three language regimes broadly corresponding to pre-crisis conventional policy, the zero-lower-bound/QE era, and post-2020 pandemic/tightening communications
- Post-COVID minutes showed lower net sentiment (0.003 vs. 0.008 pre-COVID) while uncertainty remained roughly constant across both periods, suggesting the Fed's tone shifted but its hedging patterns stayed structurally consistent

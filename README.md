# NLP Explorations

A collection of applied NLP notebooks exploring core techniques in natural language processing, with particular attention to preprocessing decisions, model comparison, and linguistic interpretation of results — not just running a method, but examining *why* it succeeds or fails on real text.

Each notebook picks a real dataset or literary text, applies one or more NLP techniques, and works through concrete cases where the method's output is linguistically interesting, surprising, or wrong — using that analysis to draw conclusions about the method's assumptions and limitations.

## What this demonstrates

- **Preprocessing decisions as an experimental variable, not a fixed step** — rather than treating tokenization/stopword removal/lemmatization as boilerplate, several notebooks isolate preprocessing choices as independent variables and measure their downstream effect on model quality (e.g. explained variance in dimensionality reduction, classification accuracy).
- **Model comparison grounded in linguistic explanation, not just metrics** — when one method underperforms another, the analysis traces the *why* to specific linguistic phenomena (e.g. multi-word expressions a bag-of-words model can't capture, subword tokenization artifacts, chunking-induced context loss) rather than stopping at the accuracy numbers.
- **Precision/recall tradeoffs tied to real-world consequence, not treated as abstractly interchangeable** — misclassifications are evaluated by the actual cost of the error in context (e.g. a missed travel alert vs. an unwanted bank transfer), matching how these tradeoffs are actually reasoned about in production systems.
- **Comparing classical and neural approaches to the same task** (spaCy's rule-based NER vs. a transformer NER pipeline) with specific attention to *how* each model processes text differently (tokenization strategy, context window/chunking limits) and how that produces systematically different errors, not just different accuracy.
- **Comfort working across the NLP toolkit stack** — spaCy, scikit-learn, Gensim, Hugging Face `datasets`/`transformers`, GloVe embeddings — applied to genuinely different problem types (dimensionality reduction, topic modeling, classification, sequence labeling).

## Notebooks

### `LSA_TF_IDF_preprocessing_comparison.ipynb`
Examines how preprocessing decisions and TF-IDF hyperparameters affect Latent Semantic Analysis's ability to compress information. Using five categories from the 20 Newsgroups dataset (including two semantically close sports categories, included deliberately as a harder disambiguation test), compares a minimal-preprocessing baseline against three variations: spaCy tokenization/lemmatization/POS-filtering only, TF-IDF frequency pruning (`min_df`/`max_df`) only, and both combined. Measures explained variance from Truncated SVD and vocabulary sparsity across all four approaches.

**Key finding:** the linguistically "cruder" approach (frequency pruning alone) outperformed strict POS filtering, because spaCy's aggressive removal of adjectives and proper nouns strips genuinely discriminative vocabulary that frequency-based pruning preserves — a concrete example of a more sophisticated preprocessing method not being automatically better for a given downstream task.

### `Intent_Classification_NB_LinearSVC_CLINC150.ipynb`
Builds and compares two text classifiers (Multinomial Naive Bayes and Linear SVM) for virtual assistant intent recognition on the CLINC150 dataset, deliberately restricted to eight semantically similar travel-domain intents plus one out-of-domain banking intent, to stress-test disambiguation.

Goes beyond aggregate accuracy to examine individual misclassifications linguistically — e.g. cases where stopword removal destroyed the specific word that disambiguated the intent (removing "when" from a distance-vs-lost-luggage query), and cases attributable to the bag-of-words assumption failing on multi-word expressions ("would like some suggestions"). Closes with a precision/recall analysis tied to the actual cost of each intent's misclassification (a missed travel alert vs. an erroneous bank transfer triggered by a false positive).

### `ner_ngram_spacy_vs_transformers_great_gatsby.ipynb`
Applies Named Entity Recognition and n-gram analysis to the full text of *The Great Gatsby*, comparing spaCy's rule-based NER against a Hugging Face transformer NER pipeline (RoBERTa) for character-mention counting.

Documents a real normalization problem in spaCy's raw output (duplicate entities from name-variant references, and the mis-tagging of possessives as `PERSON` rather than `PRON`), and after normalizing both models' outputs, finds the transformer pipeline still under-counts main characters significantly. Traces this to two causes specific to how transformers process long documents: a **chunk-boundary problem** (splitting the novel into 400-token windows to respect RoBERTa's context limit causes entities near a chunk boundary to lose the surrounding context the self-attention mechanism needs), and subword tokenization artifacts requiring extra postprocessing that spaCy's whole-document processing doesn't need. Also includes adjectival co-occurrence analysis for the protagonist and a three-way bigram/trigram comparison (raw tokens vs. lemmatized/stopword-removed) showing how stopword removal changes what a frequency analysis can reveal about narrative voice and period-specific phrasing.

### `Topic_Modeling_Doc2Vec_LinkedIn_job_postings-WIP.ipynb` *(work in progress)*
Applies unsupervised learning to a real-world LinkedIn job postings dataset: spaCy preprocessing, a Gensim dictionary/BoW/TF-IDF pipeline, LDA topic modeling, a job-matching system built on both TF-IDF cosine similarity and Doc2Vec embeddings, and exploration of pretrained GloVe embeddings for vector arithmetic and outlier detection on domain-specific vocabulary.

Includes reflective analysis on evaluating unsupervised output without ground truth (manual cluster/ranking inspection as the only real option at this scale), where GloVe's general-purpose training data (not domain-specific to job postings) breaks down for domain vector arithmetic, and a design sketch for combining LDA, Doc2Vec, TF-IDF, and GloVe into a single feature vector for a downstream supervised model.

## Tools & libraries

`spaCy`, `scikit-learn`, `Gensim`, Hugging Face `datasets` and `transformers`, `pandas`, `numpy`, pretrained GloVe embeddings, KaggleHub.

## Author

Jack Bowers
=======
A collection of applied NLP notebooks exploring core techniques in natural language processing, including, though not limited to: named entity recognition, text classification, topic modeling, and semantic retrieval. 

Each notebook applies a specific method to real-world or benchmark datasets, with attention to preprocessing decisions, model comparison, and linguistic interpretation of results.
>>>>>>> bbde02643d710c42a27ceea34b7e80c6a467dfe4
=======
A collection of applied NLP notebooks exploring core techniques in natural language processing, including, though not limited to: named entity recognition, text classification, topic modeling, and semantic retrieval. 

Each notebook applies a specific method to real-world or benchmark datasets, with attention to preprocessing decisions, model comparison, and linguistic interpretation of results.
>>>>>>> bbde02643d710c42a27ceea34b7e80c6a467dfe4

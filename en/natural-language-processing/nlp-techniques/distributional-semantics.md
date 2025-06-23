# Distributional Semantics

Based on the idea that words appearing in similar contexts have similar meanings.

1. **Skip-gram (in Word2Vec)** – _predicts context from a word_
   * Learns word embeddings that reflect word meaning.
2. **CBOW (Continuous Bag of Words)** – _predicts word from context_
   * Similar goal, different direction.
3. **GloVe (Global Vectors for Word Representation)**
   * Combines matrix factorization with co-occurrence statistics.
4. **Latent Semantic Analysis (LSA)**
   * Reduces dimensionality of word-document matrices via SVD.
5. **Latent Dirichlet Allocation (LDA)**
   * A probabilistic model for discovering topics in text.
6. **Contextual Embeddings** _(Neural-based semantics)_
   * E.g., **ELMo**, **BERT** — generate embeddings based on the entire sentence.

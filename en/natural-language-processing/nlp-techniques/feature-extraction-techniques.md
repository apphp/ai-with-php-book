# Feature Extraction Techniques

These are often used before applying statistical or ML models, especially in classical NLP.

1. **Bag of Words (BoW)**
   * Converts text into a fixed-length vector of word counts.
   * Ignores grammar and word order.
2. **Term Frequency–Inverse Document Frequency (TF-IDF)**
   * Extends BoW by weighting words by their importance across documents.
3. **n-Grams**
   * Extends BoW/TF-IDF with multi-word units (e.g., bigrams, trigrams).
   * Used both for feature extraction and as standalone statistical models.
4. **Word Embeddings**
   * Dense vector representations capturing semantic meaning.
   * Includes:
     * **Word2Vec**: Skip-gram and CBOW
     * **GloVe**: Matrix factorization-based
     * **FastText**: Includes subword information
5. **Kernel Methods (e.g., string kernels, tree kernels)**
   * Use structured similarity measures for text, often in SVMs.

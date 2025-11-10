# TF-IDF from Scratch for Duplicate Question Detection

This project provides a from-scratch implementation of the **TF-IDF (Term Frequency-Inverse Document Frequency)** algorithm paired with **Cosine Distance** to identify duplicate and near-duplicate text entries.

The primary use case for this repository is analyzing a corpus of medical examination questions to detect instances of plagiarism or content recycling, where questions are slightly modified but semantically similar.

---

## Problem Context

In the management of question banks for professional examinations, it is crucial to ensure the uniqueness and integrity of the content. This tool was developed to address the challenge of identifying questions that have been resubmitted with minor alterations—a practice that can compromise the fairness and validity of the exam.

By vectorizing questions using TF-IDF and comparing them with cosine distance, the system can effectively flag potential duplicates for manual review.

---

## Methodology

The entire process is implemented in a single Python script using standard libraries, with `NumPy` for vector operations and `PyMuPDF` for data ingestion.

1.  **Data Ingestion:**
    * All `.pdf` files are loaded from a specified folder using `PyMuPDF (fitz)`.
    * Text is extracted and concatenated into a single corpus.

2.  **Text Preprocessing:**
    * The corpus is segmented into individual questions using regular expressions (`re`) based on question markers ("Nr").
    * Each question is cleaned by:
        * Removing boilerplate text (headers, footers).
        * Stripping answer choices (e.g., "A.", "B.").
        * Removing punctuation, newlines, and extra whitespace.
        * Converting all text to lowercase.

3.  **TF-IDF Vectorization (From Scratch):**
    * **Bag-of-Words:** Each question is first converted into a `collections.Counter` dictionary, mapping each term to its raw frequency.
    * **Term Frequency (TF):** The raw term count is normalized by dividing by the total number of terms in the document (question).
    * **Inverse Document Frequency (IDF):** The IDF for each term is calculated from scratch using the formula: $log_{10}(\frac{N}{df})$, where `N` is the total number of documents and `df` is the number of documents containing the term.
    * **Final Vector:** The TF and IDF values are multiplied to produce the final TF-IDF weight for each term in each document.

4.  **Similarity Analysis (From Scratch):**
    * **Cosine Distance:** A custom function calculates the cosine distance ( $1 - \text{cosine similarity}$ ) between the TF-IDF vectors (represented as dictionaries) of every unique pair of questions.
    * This function uses `NumPy` to handle the vector math efficiently.

5.  **Duplicate Identification:**
    * The script calculates the pairwise distances for all questions.
    * It then sorts these distances and prints the top `n` pairs with the lowest distance scores, representing the most similar (i.e., likely duplicate) questions.

---

## Technology Stack

* **Python 3**
* **PyMuPDF (`fitz`):** For PDF text extraction.
* **NumPy:** For high-performance vector operations in the cosine distance calculation.
* **Standard Libraries:** `os`, `re`, `math`, `collections`, `copy`.

---

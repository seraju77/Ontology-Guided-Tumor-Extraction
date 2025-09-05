# Ontology-Guided Extraction of Tumor Characteristics from Breast Cancer Pathology Reports



This project presents an ontology-guided NLP pipeline for extracting tumor biomarkers (ER, PR, and HER2) and laboratory test information from unstructured breast cancer pathology reports. The framework leverages ClinicalBERT, fine-tuned for token-level classification, and integrates post-processing with LOINC and SNOMED CT for standardized, interoperable outputs.

Key Features
-ClinicalBERT-based Entity Extraction
 Fine-tuned transformer model for recognizing biomarkers, test values, units, and temporal expressions from free-text pathology narratives.
-Ontology-Guided Normalization
 Extracted entities are cleaned and mapped to controlled vocabularies (LOINC & SNOMED CT), ensuring compatibility with electronic health records (EHRs) and research databases.
-Attention-based Explainability
 Token-level rationales and heatmaps offer interpretable insights into model predictions, thereby enhancing clinical trustworthiness.
-Synthetic + Structured Data Training
 Combines SEER registry data (2010–2022, ~385k records) with large-scale synthetic pathology-style reports for robust training and evaluation.

Results
-Achieved weighted F1 = 1.000 and accuracy of 0.99 on held-out test sets.
-Strong generalization across common ER/PR/HER2 combinations, with acceptable performance even for rare subtypes.
-Demonstrated clear interpretability, aligning model focus with clinically relevant tokens.

Contributions
-Provides a high-performance, interoperable, and explainable information extraction system for oncology.
-Bridges gaps between free-text pathology reports and standardized data for decision support, clinical trials, and large-scale cancer research.
-Moves toward multi-institutional integration and scalable clinical deployment.

![data_preprocessing](https://github.com/user-attachments/assets/b0ea7f13-80ec-455f-bee5-77df9f96e5ce)
Figure: Structured data from the SEER database and unstructured synthetic reports are preprocessed and combined for multi-label classification tasks.

![clinicalber_model](https://github.com/user-attachments/assets/7181cb1b-b8d0-4d48-9970-ab167fe8344c)
Figure: Detailed architecture of the ClinicalBERT model used in this study. Input clinical text is tokenized with special tokens. It is transformed into embeddings with positional encoding and processed through multiple transformer encoder layers that include residual connections and normalization. The result is passed to a classification head for token-level entity prediction. The output probabilities for each entity label are computed using the softmax function, \( \hat{y}_t = \text{softmax}(W h_t + b) \), and optimized with cross-entropy loss during fine-tuning.

<img width="975" height="249" alt="toke_wise_attentation" src="https://github.com/user-attachments/assets/37cf352b-b997-4ad1-8967-626ae1224181" />
Figure: Attention-based interpretability of ClinicalBERT predictions: token-wise attention weights to [$CLS$] in a representative clinical note.


### Comparative performance of the proposed ClinicalBERT pipeline against biomedical and general-purpose transformer baselines

| Model            | MaxLen | Pooling | Eval Loss | Acc    | Prec   | Rec    | F1     | Runtime (s) | Samples/s | ΔF1 (pp) |
|------------------|--------|---------|-----------|--------|--------|--------|--------|-------------|-----------|----------|
| Bio_ClinicalBERT | 256    | CLS     | 0.0252    | 0.9957 | 0.9929 | 0.9957 | 0.9941 | 39548.31    | 0.152     | -0.59    |
| BioBERT          | 256    | CLS     | 0.0241    | 0.9960 | 0.9937 | 0.9960 | 0.9946 | 40423.02    | 0.149     | -0.54    |
| PubMedBERT       | 256    | CLS     | 0.0184    | 0.9978 | 0.9963 | 0.9978 | 0.9970 | 39120.06    | 0.154     | -0.30    |
| DeBERTa-v3       | 256    | CLS     | 0.0234    | 0.9957 | 0.9928 | 0.9957 | 0.9940 | 19669.08    | 0.305     | -0.60    |
| DistilBERT       | 256    | CLS     | 0.0294    | 0.9950 | 0.9920 | 0.9950 | 0.9940 | 20000       | 0.300     | -0.60    |
| **Proposed**     | 256    | CLS     | **0.015** | **1.0000** | **1.0000** | **1.0000** | **1.0000** | 39000       | 0.153     | **0.00** |



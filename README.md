# Applied NLP: Midterm Project — Language Modeling Architecture Comparison

## Overview
This repository contains an individual comparative study of three language modeling architectures trained from scratch on *Alice in Wonderland* (via Project Gutenberg)[cite: 11]. The objective of this project is not to build a production-scale foundation model, but rather to empirically examine how different neural and statistical architectures scale in capability, handle sequence context, and balance compute complexity.

---

## Repository Structure
- `README.md`: Project summary, architecture details, evaluation metrics, and reflections.
- `midterm_project.ipynb`: The complete executable Jupyter notebook containing data loading, training loops, generation scripts, and model evaluations.
- `ai_use_summary.md`: Summary documentation detailing responsible AI collaboration during development.

---

## 1. NLP Task Solved
All three models are trained to solve the exact same task: **Character-level Next-Character Prediction** (autoregressive language modeling), where the model takes a sequence of preceding text characters and predicts the most probable subsequent character.

---

## 2. Dataset Used
- **Source:** *Alice in Wonderland* by Lewis Carroll (retrieved via Project Gutenberg)[cite: 11].
- **Preprocessing:** Cleaned of header/footer boilerplate, mapped to a character-level vocabulary (`vocab_size = 76`), and split deterministically into a 90% training set (130,226 tokens) and a 10% validation set (14,470 tokens)[cite: 11].

---

## 3. Models Compared
1. **Bigram Model:** A frequency-based statistical transition matrix utilizing Laplace (add-one) smoothing to calculate conditional probabilities $P(w_i \mid w_{i-1})$.
2. **Non-Transformer Model (CharLSTM):** A 2-layer Recurrent Neural Network utilizing Long Short-Term Memory (LSTM) cells, operating with hidden states to retain temporal memory over sequential input blocks.
3. **Small Transformer Model:** A compact decoder-only causal Transformer featuring token embeddings, learned positional embeddings, multi-head causal self-attention blocks, layer normalization, and a GELU feedforward network.

---

## 4. Results & Evaluation Table

| Model | Train Loss | Validation Loss | Training Time | Generated Text Quality Notes |
| :--- | :--- | :--- | :--- | :--- |
| **Bigram** | — | 2.4405 | 2.81 seconds[cite: 11] | Low quality. Generates broken character strings, gibberish word clusters, and random punctuation fragments. |
| **Non-Transformer (LSTM)** | 2.1421 *(Epoch 3)*[cite: 11] | 1.9865[cite: 11] | 53.01 seconds[cite: 11] | Medium quality. Successfully captures short-term English structure, common words, proper spacing, and dialogue markers. |
| **Transformer** | 2.0546 *(Epoch 3)*[cite: 11] | 1.9296[cite: 11] | 291.29 seconds[cite: 11] | High quality. Learns complex word structures, punctuation formatting, and short phrase coherence more reliably. |

---

## 5. Qualitative Analysis & Key Findings

- **Which model performed best?** 
  The **Small Transformer** achieved the lowest validation loss (1.9296) and demonstrated the most robust structural text generation, followed closely by the LSTM.
- **Which model trained fastest?** 
  The **Bigram model** trained instantaneously (~2.8 seconds) since it is a closed-form frequency count table, whereas the LSTM and Transformer required multi-epoch gradient descent optimization.
- **Which model generated the most realistic text?** 
  The **LSTM and Transformer** both successfully captured standard English spelling rules and sentence boundaries, avoiding the completely random character pairings produced by the Bigram model.
- **What did the transformer improve compared to other models?** 
  Unlike the Bigram model (which is strictly myopic) or the LSTM (which processes sequentially and suffers from state degradation over time), the Transformer's self-attention mechanism directly evaluates relationships across tokens inside the block window.
- **Was the improvement worth the added complexity?** 
  For this tiny character-level dataset, the Transformer required roughly $5\times$ more training time than the LSTM for marginal validation loss gains. However, its architectural scalability makes it vastly superior for larger contexts.

---

## 6. Final Reflection
> **After comparing bigram, non-transformer, and transformer models, what do you think makes transformers powerful for language modeling?**  
> Transformers are powerful because self-attention eliminates the sequential recurrence bottleneck. By allowing every token in the context window to directly attend to past tokens in parallel, transformers route information efficiently across long spans, bypassing the fading memory traces that constrain traditional recurrent networks like LSTMs.

---

## 7. Responsible AI Use Summary (`ai_use_summary.md`)
- **Assistance Provided:** AI collaborators assisted in structuring the PyTorch data-batching generators, verifying PyTorch state tensor management, drafting markdown reporting templates, and troubleshooting shape mismatch issues.
- **Independent Understanding:** All core architectural concepts (self-attention matrix masking, causal relationships, gradient backpropagation through time, and cross-entropy loss optimization) were thoroughly reviewed, debugged, and understood independently.

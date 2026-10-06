[[Usefull_Websites]]

## Tokenization

**Tokenization is the process of converting raw text into smaller units called tokens so that a language model can process the text as numbers.**

An LLM does **not directly understand or process words, sentences, or characters**. Its neural network ultimately works with numerical representations of tokens.

---

### 1. Raw text

Suppose we have:

```
I love machine learning.
```

A tokenizer might split this into:

```
["I", " love", " machine", " learning", "."]
```

Notice that a token doesn't necessarily equal a complete word.

For example:

```
"unbelievable"
```

could be split into something conceptually like:

```
["un", "believ", "able"]
```

The exact split depends on the tokenizer and its vocabulary.

---
### 2. Tokens are converted into IDs

Each token has an ID in the tokenizer's vocabulary.

For example, imagine:

```
"I"        → 40
" love"    → 1847
" machine" → 7632
" learning"→ 4219
"."        → 13
```

So:

```
I love machine learning.
```

becomes:

```
[40, 1847, 7632, 4219, 13]
```

These integer IDs are what are passed into the model.

---

### 3. Why not just use words?

Because a language has an enormous number of possible words and word forms.

Consider:

```
run
runs
running
runner
runners
```

Instead of having a separate token for every possible word, subword tokenization can represent words using reusable pieces.

This also helps with **unknown or rare words**.

For example:

```
"microservices"
```

might be represented using several known pieces rather than requiring `"microservices"` to exist as one vocabulary entry.

---

### 4. What happens after tokenization?

The token IDs are converted into **vectors called embeddings**.

Simplified:

![[tokenization.excalidraw]]

For example:

```
"I love AI"
     ↓
[40, 1847, 912]
     ↓
[embedding vector]
     ↓
Transformer
```

The embedding vectors contain learned numerical representations that the Transformer can work with.

---

### 5. How does this connect to next-token prediction?

Suppose the input is:

```
The capital of France is
```

After tokenization:

```
[The] [capital] [of] [France] [is]
```

The model processes these tokens and predicts a probability distribution for the **next token**:

```
Paris    → 0.92
London   → 0.01
Berlin   → 0.01
Rome     → 0.01
...
```

The target/actual next token is:

```
Paris
```

The model calculates its loss based on how much probability it assigned to the correct token.

Then:

```
Prediction
    ↓
Loss
    ↓
Backpropagation
    ↓
Weight update
```

This process happens over and over during pre-training.

---

### 6. Tokens are not always words

This is the most important thing to understand.

A token can be:

```
Complete word:
"cat"

Part of a word:
"ing"

Punctuation:
"."

Whitespace + word:
" hello"

Special token:
<|endoftext|>
```

Therefore:

> **Token ≠ word**

A sentence containing 10 words might contain 12–15 tokens, depending on the tokenizer.

---

### 7. Why tokenization matters

Tokenization affects several important things:

- **Context length** — models have limits measured in tokens.
- **Training cost** — more tokens means more computation.
- **Language efficiency** — some languages may require more tokens than others.
- **Code processing** — programming syntax is also tokenized.
- **Model vocabulary** — determines which pieces the model can efficiently represent.

For example, if a model has a context window of **128K tokens**, that means the model can process roughly 128,000 tokens in that context—not necessarily 128,000 words.

---

## Summary

**Tokenization** is the process of breaking raw text into smaller units called **tokens** that an LLM can process.

Example:

```
"I love machine learning."
        ↓
["I", " love", " machine", " learning", "."]
        ↓
[40, 1847, 7632, 4219, 13]
```

Key points:

- A **token is not necessarily a complete word**; it can be a word, part of a word, punctuation, or special symbol.
- Each token is mapped to a unique **token ID**.
- Token IDs are converted into **embeddings (vectors)** before entering the Transformer.
- During pre-training, the model uses these tokens to **predict the next token**.
- Tokenization affects **context length, training cost, and how efficiently different languages and types of text are represented**.

**In one line:**

> **Tokenization converts human-readable text into numerical pieces that an LLM can process and learn from.**

---

## The End
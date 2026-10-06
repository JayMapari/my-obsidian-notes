[[Usefull_Websites]]
# What exactly is pre-training?

Suppose we want to build a model like GPT.

Before the model can answer:

> "Explain binary search in simple terms."

it first needs to learn things like:

- What words mean
- How sentences are structured
- Grammar
- Programming syntax
- Mathematical patterns
- Facts and relationships
- Different writing styles
- How concepts relate to each other
- How to predict what comes next in a sequence

This initial learning process is called **pre-training**.

The important point is:

> **The model isn't initially taught answers one by one. It learns statistical patterns from enormous amounts of text.**

For a typical autoregressive LLM, the fundamental task is:

> **Given previous tokens, predict the next token.**

![[pretraining.excalidraw]]


---

# 1. Data Collection

The first problem is:

> **Where do we get the training data?**

LLMs need enormous quantities of data.

Sources can include:
- Websites
- Books
- Wikipedia
- Scientific papers
- Documentation
- Programming repositories
- News/publications
- Public-domain material
- Licensed datasets
- Human-generated datasets
- Synthetic data

For example, datasets such as **FineWeb** are created by processing large-scale web data into a form suitable for language-model training.

The raw data might look like:

```
HTML page
    ↓
Navigation menus
Advertisements
Cookie notices
Comments
Main article
Tracking information
Footer
```

---

# 2. Data Cleaning

Raw internet data is extremely messy.

Imagine crawling a website and getting:

```
Buy cheap shoes!!!

CLICK HERE
CLICK HERE
CLICK HERE

Advertisement
Advertisement

The actual article...

Cookie Policy
Privacy Policy
```

Training on too much low-quality material wastes computational resources.

So data pipelines perform various cleaning operations.

Common operations include:

### Remove malformed documents

For example:

```
������� � � ����
```

or corrupted HTML.

### Remove boilerplate

Things like:

```
Login
Subscribe
Cookie settings
Navigation
Footer
```

may be removed when they don't contribute useful information.

### Language identification

A dataset may contain:

```
English
Hindi
Japanese
French
Spanish
...
```

A project may decide to keep a specific mixture of languages.

---

# 3. Data Filtering

Not every piece of internet content should become training data.

Filtering can remove things such as:

- Spam
- Malware-related content
- Extremely low-quality pages
- Automatically generated junk
- Excessive advertisements
- Duplicate boilerplate
- Certain unsafe content
- Irrelevant content

A quality classifier can assign a score:

```
Document A → 0.98 quality
Document B → 0.76 quality
Document C → 0.08 quality
```

The pipeline might keep A and B while removing C.

More sophisticated pipelines can use classifiers trained specifically for data quality.

---

# 4. Deduplication

This is one of the **very important parts of LLM data preparation**.

The internet contains enormous amounts of duplicated content.

For example:

```
Wikipedia article
        ↓
Copied on Website A
        ↓
Copied on Website B
        ↓
Copied on Website C
```

If the same text appears thousands of times, the model may effectively see that information repeatedly.

This creates several problems:

- Wastes training compute
- Can distort the data distribution
- May cause memorization
- Can make evaluation misleading

Therefore, training pipelines perform **deduplication**.

There are different levels:

### Exact deduplication

Remove documents that are literally identical.

```
Document A:
"Machine learning is..."

Document B:
"Machine learning is..."
```

Keep one.

### Near-duplicate deduplication

Consider:

```
Machine learning is a field of AI.

Machine learning is an area of artificial intelligence.
```

These aren't identical but are very similar.

Techniques such as hashes, n-gram matching, MinHash, locality-sensitive hashing, and embeddings can help identify similar documents.

---

# 5. PII Removal

Another important stage is handling **personally identifiable information (PII)**.

Examples could include:

```
Full names
Private addresses
Phone numbers
Email addresses
Government identification numbers
Financial information
```

Data-processing pipelines may use:

- Pattern matching
- Regular expressions
- Named Entity Recognition
- Specialized PII detection models
- Blocklists
- Other privacy filters

For example:

```
My phone number is 9876543210
```

could potentially become:

```
My phone number is [REDACTED]
```

The exact privacy strategy varies considerably between datasets and organizations.

---

# 6. Data Quality Scoring

After basic cleaning, datasets can be assigned quality scores.

For example:

|Document|Quality|
|---|---|
|Scientific article|0.98|
|Good technical documentation|0.95|
|Wikipedia article|0.93|
|Normal blog|0.75|
|Spam page|0.05|

Then the training mixture can be controlled.

This is important because:

> **More data isn't automatically better.**

A trillion low-quality tokens aren't necessarily better than a smaller amount of carefully selected high-quality data.

Modern LLM training therefore focuses heavily on **data quality and data mixture**.

---

# 7. Data Mixture

A model usually doesn't train on just one source.

Imagine a simplified dataset:

```
Web             60%
Books           10%
Code            15%
Wikipedia        5%
Scientific text  5%
Other            5%
```

This is called the **data mixture**.

The exact percentages depend on the model.

Why does this matter?

Because different datasets teach different capabilities.

### Books

Can provide:

- Long-form reasoning
- Narrative structure
- Rich vocabulary
- Writing styles

### Code

Can provide:

- Programming syntax
- Logical structures
- APIs
- Algorithms

### Scientific papers

Can provide:

- Technical vocabulary
- Mathematical language
- Scientific relationships

### Web pages

Provide enormous diversity.

So dataset composition strongly influences what the model learns.

---

## The End 
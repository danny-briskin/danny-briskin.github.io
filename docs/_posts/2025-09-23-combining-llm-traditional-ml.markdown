---
layout: post
title: "Efficient Test Discovery in Large Codebases: Combining LLMs with Classical ML Algorithms"
date:   2025-09-23 04:40:30 -0400
categories: llm 
tags: llm, AI, agents, ML, Algorithms
---
![](/images/changelog.jpg)

Danny Briskin, Quality Engineering Practice Manager

## The Challenge: Limited Context Windows  
Large Language Models (LLMs) such as GPT are reasoning-powerful, yet only have a limited context window (say, 8k-128k tokens depending on the model).  

In **test automation** in real-world scenarios, this is a bottleneck. Suppose you have **tens of thousands of automated tests** in your codebase.  

If a user says:  
> *"I need to test the login area with multiple failed attempts"*  

you want to return the most relevant tests that already exist. However, you can’t just hose the entire test suite into the LLM - there isn't sufficient memory.

So, what do we do?
We combine **classical ML/NLP algorithms** with LLMs to **pre-filter** first and then **reason**.  


## Step 1 – Topic Detection
First, we extract the **topic** from the user’s query. This helps us match the query with the structure of existing tests.  

Options:  
- **[TF-IDF, Term Frequency-Inverse Document Frequency](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) / keyword extraction** – rapid keyword-based filtering.  
- **Embeddings** – deeper semantic understanding. 
- a single query to a LLM can be used as well at this point  

```python
from sklearn.feature_extraction.text import TfidfVectorizer

queries = ["I need to test the login area with multiple failed attempts"]
tests = ["test_login_success", "test_login_failed_attempts",
         "test_password_reset", "test_account_lockout"]

vectorizer = TfidfVectorizer()
X = vectorizer.fit_transform(tests + queries)

# Compute cosine similarity between query and tests
from sklearn.metrics.pairwise import cosine_similarity
cos_sim = cosine_similarity(X[-1], X[:-1])
print(cos_sim)
```
This gives us a first-pass ranking: **“test_login_failed_attempts”** is the winner.

## Step 2 – Candidate Filtering
Knowing that the query subject is *“login/authentication”*, we filter the repository.  

Algorithms:  
- **Cosine similarity on embeddings** → top semantic match.  
- **Longest Common Subsequence (LCS)** → useful if test names follow conventions.  
- **Edit Distance (Levenshtein)** → identifies typos in queries/test names.  

Example with embeddings:  

```python
from sentence_transformers import SentenceTransformer, util
import torch

model = SentenceTransformer('all-MiniLM-L6-v2')

query_embedding = model.encode("login failed attempts", convert_to_tensor=True)
test_embeddings = model.encode(tests, convert_to_tensor=True)

# Semantic similarity
cos_scores = util.pytorch_cos_sim(query_embedding, test_embeddings)[0]
top_results = torch.topk(cos_scores, k=3)

for idx in top_results.indices:
    print(tests[idx])
```

Output:  
```
test_login_failed_attempts
test_account_lockout
test_login_success
```

Now we have **candidates** instead of the entire repository.  

## Step 3 – LLM Refinement
Finally, only run candidate tests through the LLM and allow it to **refine, filter, and suggest enhancements**:  

**Prompt Example:**  
```
User wants: "I need to test the login area with multiple failed attempts."

Candidate tests:
1. test_login_failed_attempts
2. test_account_lockout
3. test_login_success

Question: Which tests are most relevant? Suggest if any coverage is missing.
```

**LLM Output:**  
- Most relevant: `test_login_failed_attempts`, `test_account_lockout`.  
- Less relevant: `test_login_success`.  
- Missing: A test for captcha bypass after repeated failures.  


## The Hybrid Workflow
Putting it together:

```
+------------------+
|   User Query     |
+------------------+
         |
         v
+------------------+
| Topic Detection  |
| (TF-IDF, Embed.) |
+------------------+
         |
         v
+---------------------------+
| Candidate Filtering       |
| (Cosine, LCS, Edit Dist.) |
+---------------------------+
         |
         v
+------------------+
|  LLM Refinement  |
+------------------+
         |
         v
+---------------------------+
| Suggested Relevant Tests  |
+---------------------------+
```

This approach:  
- Maintains LLM inputs small.  
- Ensures fast retrieval.  
- Trades off on deterministic filtering and generative reasoning.  

## Why Not Just Use the LLM Alone?
- **Context window limit** – you can’t put the whole test suite in.  
- **Efficiency matters** – looping all tests through the LLM is expensive.  
- **Reliability** – algorithms provide deterministic filtering before involving the LLM.  


## Generalizing Beyond Tests
This pattern applies in many contexts:  
- Knowledge base search (support tickets, FAQs).  
- Document retrieval (legal, medical, financial).  
- Conversational memory management.  


## Conclusion
Traditional ML/NLP algorithms are still important - they make LLMs practical in real-world systems.  

In our test automation example, cosine similarity and sequence matching help narrow down thousands of tests to a few. The LLM can then reason over these results.  

The result:  
- Faster.  
- Cheaper.  
- More accurate.  

Hybrid systems that use traditional algorithms for retrieval and LLMs for reasoning are the future of agent design.  
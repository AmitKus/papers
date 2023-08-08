# Embedding papers

## E5 embeddings

Paper: [Text Embeddings by Weakly-Supervised Contrastive Pre-training](https://arxiv.org/pdf/2212.03533.pdf)

### Abstract:
- a family of state-of-the-art text embeddings that transfer well to a wide range of tasks. 
- trained in a contrastive manner with weak supervision signals from our curated large-scale text pair dataset (called CCPairs). 
- useful for any tasks requiring a single-vector representation of texts 
    - retrieval, clustering, and classification, 
    - achieving strong performance in both zero-shot and fine-tuned settings. 
- For zero-shot settings, E5 is the first model that outperforms the strong BM25 baseline on the BEIR retrieval benchmark without using any labeled data. 
- When fine-tuned, E5 obtains the best results on the MTEB benchmark, beating existing embedding models with 40× more parameters.

### Notes

- E5: EmbEddings from bidirEctional Encoder rEpresentations.
- To obtain better text embeddings, contrastive learning is often the go-to framework to enhance the sequence-level representations from text pairs.
- instead of relying on limited labeled data or low-quality synthetic text pairs, we contrastively train E5 embeddings from CCPairs, a curated web-scale text pair dataset containing heterogeneous training signals.
- Contrastive loss popularized by SimCLR [10] turns out to be more effective than classificationbased losses for embeddings.
- LaBSE [20], LASER [2] and CLIP [47] further extend to multilingual and multi-modal scenarios using parallel sentences and image-text pairs.
- Self-supervised pretraining methods:
    - inverse cloze task (ICT): a random sentence within a passage is chosen as a pseudo-query and the rest is treated as a positive sample.
    - Contriever [28] shows that random cropping with data augmentation is more effective than ICT on a range of zero-shot information retrieval tasks.
    - OpenAI text embeddings [41] use neighboring texts as positives and scale up the model size to 175B.
    - Oguz et al. [45] performs domain-matched pre-training to improve in-domain results.
    - Issue: **synthetic data tend to be of low quality**.

### CCPairs: A Large Collection of Text Pair Dataset
- (q, p) denote a text pair consisting of a query q and a passage p.
    - (post, comment) pairs from Reddit 3, 
    - (question, upvoted answer) pairs from Stackexchange 4, 
    - (entity name + section title, passage) pairs from English Wikipedia, 
    - (title, abstract) and citation pairs from Scientific papers
    - (title, passage) pairs from Common Crawl 5 web pages and various News sources.
- Only include datasets that can be mined automatically
- Consistency-based filter: a model is first trained on the 1.3B noisy text pairs, and then used to rank each pair against a pool of 1 million random passages.
    - A text pair is kept only if it falls in the top-k ranked lists. (k=2)
    - Intution: when trained on noisy datasets, neural networks tend to memorize the clean labels first and then gradually overfit the noisy labels.

![](../pics/Screenshot%20from%202023-08-08%2014-46-34.png)

### Contrastive pretraining

**Loss:** 
- $min\ L_{cont} = -\frac{1}{n}\sum \frac{e^{s_{\theta}(q_i,p_i)}}{e^{s_{\theta}(q_i,p_i)} + \sum_j e^{s_{\theta}(q_i,p_j)}}$

Where $s_{\theta}(q,p)$ is a scoring function between query q and passage p parameterized by θ. 

Following the popular biencoder architecture, we use a pre-trained Transformer encoder and average pooling over the output layer to get fixed-size text embeddings Eq and Ep. 

**Scoring function:**

The score is the cosine similarity scaled by a temperature hyperparameter τ :
$s_{\theta}(q,p) = cos(E_q, E_p)/\tau$

**Negative samples**

Another critical issue for contrastive training is how to select the negative samples. Here we choose to use the in-batch negatives, where the passages from other pairs in a batch serve as negative samples.

### Fine-tuning with labeled data

While contrastive pre-training on the CCPairs provides a solid foundation for general-purpose embeddings, further training on labeled data can inject human knowledge into the model to boost the performance.

### Applications to text embedding task

**Zero-shot Retrieval First**, the passage embeddings for the target corpus are computed and indexed offline. Then for each query, we compute its query embedding and return the top-k ranked lists from the corpus based on cosine similarity.

**Few-shot Text Classification** A linear classifier is trained on top of the frozen embeddings with a few labeled examples. Different tasks only need to train and save the parameters of the classification heads. It can be seen as a particular form of parameter-efficient learning

**Semantic Textual Similarity** Given two text embeddings, we use the cosine function to measure their semantic similarity. Since the absolute similarity scores do not enable an easy interpretation, the evaluation is usually based on rank correlation coefficients.

**Text Clustering** Standard clustering algorithms such as k-means can be applied straightforwardly. Texts belonging to the same category are expected to be close in the embedding space.

### Experiments

**Pretraining**: We pre-train on our proposed text pair dataset for three model sizes:

| Embedding model      | Embedding model |
| :---        |    :----:   |
| E5small  | MiniLM | 
| E5base | bert-base-uncased |
| E5large | bert-large-uncased-whole-wordmasking |

![](../pics/Screenshot%20from%202023-08-08%2014-49-11.png)

- Large batch size (to increase #negatives): 32,768

**Fine-tuning**
- Datasets: MS-MARCO passage ranking, NQ, and NLI
- Batch size: 256
- Negatives: 7 hard negatives

### Evaluation datasets

**BEIR Benchmark**: 
- A collection of 19 information retrieval datasets, ranging across ad-hoc web search, question answering, fact verification and duplicate question retrieval, etc.

**MTEB Benchmark:**
- Recently proposed for benchmarking massive text embedding tasks. 
- Though MTEB is multilingual due to the inclusion of bitext mining datasets, most datasets are still only available in English. 
- In this paper, we evaluate the English subsets, which have 56 datasets spanning across 6 categories: Classification (Class.), Clustering (Clust.), Pair Classification (PairClass.), Rerank, Retrieval (Retr.), STS, and Summarization (Summ.). 
- The evaluation metrics are accuracy, v-measure, average precision, MAP, nDCG@10, and Spearman coefficients, respectively.

### Analysis

**Impact of Batch size**
- larger batch size will provide more negatives and therefore improve the quality of the learned text embeddings.
![](../pics/Screenshot%20from%202023-08-08%2014-36-54.png)

**BM25 vs Dense Retrieval** 
- With the rapid development of dense retrieval models, can we replace the long-standing BM25 algorithm from now on? 
- The answer is likely “not yet”. BM25 still holds obvious advantages in terms of simplicity, efficiency, and interpretability. For long-tail domains such as Trec-Covid [55] and retrieval tasks that involve long documents (Touche-2020) [4] or rely heavily on exact lexical match (Fever) [54], further research efforts are still necessary to improve current dense retrievers.

### Claims
- It is possible to train high-quality embeddings using self-supervised pre-training only.

    

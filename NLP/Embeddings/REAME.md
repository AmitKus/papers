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


### Claims
- It is possible to train high-quality embeddings using self-supervised pre-training only.

    

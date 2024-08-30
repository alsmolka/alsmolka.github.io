---
layout: page
title: Looking again into monolingual sentence alignment
description: New LLM metrics in monoligual sentence alignment problem
img: 
importance: 1
category: LLMs and their applications
related_publications: true

toc:
    sidebar: left

---
<blockquote>
<ul>
    <li>Aren't there any better similarity metrics for text alignment other that character ngam overlap, still used today as SOTA?</li>
    <li>How to develop an adequate NN similarity measure for monolingual sentence alignment?</li>
</ul>
</blockquote>

In this project, we were motivated by the need to align sentences in a monolingual paraphrase corpus into one-to-one mappings. As it turned out, the problem was not as trivial as it might have looked, since most SOTA approaches still used obsolate metrics such as ngram overlap to derive the alignments. Therefore, we conducted our own study to find how modern deep learning models can better answer this problem. You can check out the whole study in our paper [Is Character Trigram Overlapping Ratio Still the Best Similarity Measure for Aligning Sentences in a Paraphrased Corpus? (Smolka et al., ROCLING 2022)](https://aclanthology.org/2022.rocling-1.7.pdf) and the related [github repo](https://github.com/alsmolka/sentence-alignment).

## What is this project about?<a name="introduction"></a>

Our projects proposes several new LLM-based sentence similarity metrics which we then adopt together with two search mechanisms - a directional best match and a sequence match. Together they are used to identify one-to-one alignments between two bodies of text such as in the figure below.
[figure]

The best match approach is similar to a greedy search. We use precomputed similarity scores (based on one of the metrics) and link sentences which have the highest similarity. We extract such pairs starting from those most similar ones. We also test a uni-directional and bi-directional version of this alogrithm. In one case, we only care about the similarity mapping from the first text to the second. In the second case, we take into consideration the similarity ranking in both directions between the texts.

Another algorithm we test for the alignment with our various metrics is a sequential match which is a popular dynamic programming approach to sequence alignment (Gale and Church, 1993).

## Which metrics did we test?<a name="models"></a>

The metrics we tested all belong to one of two main categories - either string-based similarity measures or embedding-based similarity measures. The string-based similarity measures are for example, measuring the token or character ngram overlap between sentences. The embedding-based metrics are of a bigger interest to us. To obtain such measure, we usually first embed both sentences using a selected LLM (we tested BERT-family models) and calculate the cosine similarity between the embeddings. Another option is to use a premade metric such as BERTScore.


## What is the best similarity metric?<a name="results"></a>

In the end, it turned out that, unsuprisingly, the best results were achieved with transformer model embeddings, though the results differed slightly for different alignment algorithm. If you'd like to have a look at full results or read about all the measures we tested, definitely check out the paper linked at the top of the page!
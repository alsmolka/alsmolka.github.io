---
layout: page
title: Make the music suit your moody lyrics!
description: Effect of different types of features on conditioning a music generation transformer.
img: 
importance: 2
category: Music & Speech
giscus_comments: false

toc:
    sidebar: left
mermaid:
  enabled: true
  zoomable: true
chart:
  echarts: true
  chartjs: true
---
<blockquote>
<ul>
    <li>How can we attune the model so that the mood of the generated music fits the mood of the lyrics used as prompt?</li>
    <li>What type of features (textual, categorical, numerical) is best for this task?</li>
</ul>
</blockquote>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/music_gen.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


## What is this project about?

We experimented with a method of combining different features obtained from lyrics, trying to create a music generation model (with a transformer backbone) that will create melodies matching the mood of the lyrics. The full technical report can be downloaded [here](https://drive.google.com/file/d/15GsT2maLhxM2k_siUYmDCTUcvOq_8X3T/view?usp=sharing).


## Training data

We collected our music samples based on several criteria:
- they were songs with lyrics, not instrumental tracks (as otherwise how would we condition our model on lyrics content);
- the samples were in MIDI format (since our model was to be trained from scratch it made it more time- and resource-effective);
- there were lyrics available for each of the samples (so that we do not need to automatically generate them);
- there are additional metadata such as artist or genre (to experiment with different song features other than textual);
- the size of dataset after sample collection is sufficient (transfromers are quite data-greedy when usual pre-training methods are used).

At the time of the study, the dataset best matching our needs was [FMA dataset]() designed for music analysis. The FMA dataset contains mp3 samples, which we converted to MIDI using audio-to-midi Python library and crawled the web to match each sample with lyrics.


## Feature extraction

Feature-engineering involved three types of features: lyric-based, general metadata and audio information. You can find a list of the features below, with more specific explainations in the file linked at the top of the page.

> <font size="5">LYRIC_BASED FEATURES</font>
>
>- list of keywords (tf-idf)
>- topic (top2vec model)
>- emotions distribution (text2emotion package)

> <font size="5">GENERAL METADATA</font>
>
>- genre
>- artist bio
>- location
>- keywords
>- date of album release
>- album engineer
>- album producer
>- album popularity
>- album type
>- artist image
>- artist location
>- artist members

> <font size="5">AUDIO METADATA</font>
>
>- acousticness
>- danceability
>- energy
>- instrumentalness
>- liveness
>- speechiness
>- tempo
>- valence
>- song hotness

## Model training
We first train a transfomer baseline, which generates music trained only on MIDI files (without any additional features). For our proposed model, we train several variants: 

1) using distributional emotion features (as additional embedding)
2) with scaled musis analysis features
3) using all listed textual features
4) using genre as a single categorical feature

## Evaluation method

We used both an automatic and manual evaluation to assess the generated music samples. For the automatic evaluation, we combine two modules to separatedly retrieve the emotions from text used for prompting and from the associated generated music sample. We then compare the two, with cases where the two emotions match regarded as success. For the human evaluation, we prepared a special survey in which the annotators are asked to rate how well the music sample matches the lyrics they were provided.

## Results

For the automatic evaluation, the baseline accuracy was around 65%. Our lyrics-conditioned models scored lower, at around 35%. However, when it comes to human evaluation our model beat the baseline by 7% (39.5% vs. 46.5%).

In the end, the biggest limitation of our model was its preliminary character and the memory limitations for the GPUs we had avaiable at that time. This made it difficult to train a high-performing music generation model, regardless whether it was conditioned on additional lyrics or in its pure form. Some other issues could be tied to the relevance of the performance measure which could be reasessed and the feature extraction methods, as there are many more successful approaches to it nowadays. Still, our model was able to generate music which sounded closer to the mood of the provided lyrics, which was its main purpose.

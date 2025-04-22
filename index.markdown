---
layout: home
title: MuseTok
description: Symbolic Music Tokenization for Hierarchical Generation and Semantic Understanding
---

# Introduction

In this paper, we propose **MuseTok**, a tokenization method for symbolic music, and investigate its effectiveness in both music generation and understanding tasks. MuseTok employs RQ-VAE on bar-wise music segments within a transformer-based encoder-decoder framework, producing music codes that achieve **high-fidelity music reconstruction** and **accurate understanding of music theory**. We further applied learned MuseTok to **music generation** and **semantic understanding** tasks, to comprehensively evaluate the quality of such representations.

<div align="center">
  <img src="figures/overview.png" width=800 alt="">
  <figcaption><strong>Fig.1</strong> Overview of MuseTok (left) and downstream generation (top right) and understanding tasks (bottom right).</figcaption>
</div>

We represent audio demos and additional experiment results as follows to supplement some sections in the paper:
* Dataset (for Section 4.1)
* Music Reconstruction (for Section 4.2)
* Music Continuation (for Section 4.3)
* How MuseTok Learns Music? (for Section 4.5)

# Dataset

We utilize public-domain music data for model training as follows, with the representative samples shown for present the data characteristics of our mixed dataset:

<table class="audio-table">
  <thead>
    <tr class="header">
    <th>Dataset</th>
    <th># pieces</th>
    <th>Genres</th>
    <th>Sample1</th>
    <th>Sample2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>PDMX-monophonic [1]</td>
      <td>163,366</td>
      <td>mixed</td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/PDMX-mono_1.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/PDMX-mono_2.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td>PDMX-contrapuntal [1]</td>
      <td>25,597</td>
      <td>mixed</td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/PDMX-contra_1.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/PDMX-contra_2.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td>POP909 [2]</td>
      <td>886</td>
      <td>pop</td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/pop909_1.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/pop909_2.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td>EMOPIA [3]</td>
      <td>1,071</td>
      <td>pop</td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/emopia_1.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/emopia_2.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td>Pop1k7 [4]</td>
      <td>1,747</td>
      <td>pop</td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/pop1k7_1.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/pop1k7_2.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td>Hymnal [5]</td>
      <td>1,606</td>
      <td>folk</td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/Hymnal_1.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/Hymnal_2.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td>Multipianomide [6]</td>
      <td>457</td>
      <td>classical</td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/Multipianomide_1.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/Multipianomide_2.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td>Ragtime [7]</td>
      <td>457</td>
      <td>jazz</td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/ragtime_1.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/dataset_demo/ragtime_2.wav" type="audio/mpeg" /></audio></td>
    </tr>
  </tbody>
</table>

**Reference:**

[1] P. Long, Z. Novack, T. Berg-Kirkpatrick, and J. J. McAuley, “PDMX: A large-scale public domain musicxml dataset for symbolic music processing,” in Proc. ICASSP, 2025.

[2] Z. Wang, K. Chen, J. Jiang, Y. Zhang, M. Xu, S. Dai, and G. Xia, “POP909: A pop-song dataset for music arrangement generation,” in Proc. ISMIR, 2020.

[3] H. Hung, J. Ching, S. Doh, N. Kim, J. Nam, and Y.-H. Yang, “EMOPIA: A multi-modal pop piano dataset for emotion recognition and emotion-based music generation,” in Proc. ISMIR, 2021.

[4] W. Hsiao, J. Liu, Y. Yeh, and Y.-H. Yang, “Compound Word Transformer: Learning to compose full song music over dynamic directed hypergraphs,” in Proc. AAAI, 2021.

[5] [https://www.hymnal.net/en/home](https://www.hymnal.net/en/home)

[6] [http://piano-midi.de/](http://piano-midi.de/)

[7] [https://www.ragsrag.com/pr/pr.html](https://www.ragsrag.com/pr/pr.html)


# Music Reconstruction

We show some samples below to demonstrate the reconstruction performance of three models (**VAE**, **MuseTok-Small** and **MuseTok-Large**) comparing to the **Original** pieces (d: hidden dimension, D: quantization depth, K: codebook size):

* **VAE** (d = 128)
* **MuseTok-Small** (D = 8, K = 1024, d = 128)
* **MuseTok-Large** (D = 16, K = 2048, d = 128)

<table class="audio-table">
  <thead>
    <tr class="header">
    <th>Original</th>
    <th>VAE</th>
    <th>MuseTok-Small</th>
    <th>MuseTok-Large</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/original/id5_bar0_orig.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/vae/id5_bar0.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/musetok-small/id5_bar0.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/musetok-large/id5_bar0.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/original/id19292_bar0_orig.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/vae/id19292_bar0.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/musetok-small/id19292_bar0.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/musetok-large/id19292_bar0.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/original/id19336_bar0_orig.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/vae/id19336_bar0.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/musetok-small/id19336_bar0.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/musetok-large/id19336_bar0.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/original/id25_bar0_orig.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/vae/id25_bar0.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/musetok-small/id25_bar0.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/musetok-large/id25_bar0.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/original/id153_bar0_orig.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/vae/id153_bar0.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/musetok-small/id153_bar0.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/reconstruction_demo/musetok-large/id153_bar0.wav" type="audio/mpeg" /></audio></td>
    </tr>
  </tbody>
</table>

# Music Continuation

To assess the performance of our model (**MuseTok-Large**) and two baselines (**REMI** and **AMT**) on the music continuation task, we conduct online listening tests to collect human assessment on the generation quality. (Please refer to the paper for more details about objective evaluation.)

After listening to the primer, participants are responsible to rate the generated continuations in a 5-point Likert scale for the following questions:

* **Pitch**: Are there any notes that sound out of key (0 score), or align with each other (5 score)regardless of the primer?
* **Structure**: How well does the generated part follow the rhythm and structure of the primer?
* **Harmony**: How well does the generated part align with the chord progression of the primer?
* **Development**: How coherent and engaging is the musical flow and motif development following the primer?

A total of 24 responses are collected, with each response containing 8 groups of generated samples randomly selected from 32 primers in the test set, resulting in 192 ratings per model for each aspect. 

The generation performance on objective and subjective metrics is presented below:

<div align="center">
  <img src="figures/subjective.png" width=800 alt="">
  <figcaption><strong>Fig.2</strong> Objective and subjective evaluations on music generation.</figcaption>
</div>

Some music primers and continuation samples presented to users during the listening test are shown below:

<table class="audio-table">
  <thead>
    <tr class="header">
    <th>Primer</th>
    <th>MuseTok</th>
    <th>REMI</th>
    <th>AMT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><audio controls=""><source src="assets/demos/continuation_demo/primer_96.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/MuseTok_96.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/REMI_96.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/AMT_96.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td><audio controls=""><source src="assets/demos/continuation_demo/primer_19033.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/MuseTok_19033.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/REMI_19033.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/AMT_19033.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td><audio controls=""><source src="assets/demos/continuation_demo/primer_180.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/MuseTok_180.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/REMI_180.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/AMT_180.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td><audio controls=""><source src="assets/demos/continuation_demo/primer_19074.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/MuseTok_19074.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/REMI_19074.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/AMT_19074.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td><audio controls=""><source src="assets/demos/continuation_demo/primer_19100.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/MuseTok_19100.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/REMI_19100.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/AMT_19100.wav" type="audio/mpeg" /></audio></td>
    </tr>
  </tbody>
</table>

When handling long-context primers, MuseTok demonstrates greater robustness comparing to REMI-like generation framework.

<table class="audio-table">
  <thead>
    <tr class="header">
    <th>Primer</th>
    <th>MuseTok</th>
    <th>REMI</th>
    <th>AMT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><audio controls=""><source src="assets/demos/continuation_demo/primer_13.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/MuseTok_13.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/REMI_13.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/AMT_13.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td><audio controls=""><source src="assets/demos/continuation_demo/primer_19074.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/MuseTok_19074.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/REMI_19074.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/AMT_19074.wav" type="audio/mpeg" /></audio></td>
    </tr>
    <tr>
      <td><audio controls=""><source src="assets/demos/continuation_demo/primer_19311.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/MuseTok_19311.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/REMI_19311.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/continuation_demo/AMT_19311.wav" type="audio/mpeg" /></audio></td>
    </tr>
  </tbody>
</table>


We also present generation samples from MuseTok without primers here to comprehensively show its effectiveness on content generation. 

<table class="audio-table">
  <tbody>
    <tr>
      <td><audio controls=""><source src="assets/demos/generation_demo/example_n8_s1024_d128.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/generation_demo/id15_sample01_temp1.2_p0.9.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/generation_demo/id16_sample01_temp1.1_p0.8.wav" type="audio/mpeg" /></audio></td>
      <td><audio controls=""><source src="assets/demos/generation_demo/id5_sample01_temp1.1_p0.9_k50.wav" type="audio/mpeg" /></audio></td>
    </tr>
  </tbody>
</table>

# How MuseTok Learns Music?

To further discover the music concepts learned through MuseTok, we examine the the usage frequency of codes across different **music textures** and **time signatures**. Here we show the frequency of the 50 most used codes in each texture group or time signature group across **all** eight quantization depths. 

## Music Textures

The top-50 frequently used codes in different texture groups are largely distinct at all layers.

<div align="center">
  <img src="figures/code_usage_texture.png" width=600 alt="">
  <figcaption><strong>Fig.3</strong> Top-50 used codes across three texture groups in all quantization depths.</figcaption>
</div>


## Time Signatures

MuseTok almost omits the time signature difference in the first codebook, but gradually diverges in deeper codebooks.

<div align="center">
  <img src="figures/code_usage_time.png" width=600 alt="">
  <figcaption><strong>Fig.4</strong> Top-50 used codes across six time signatures in all quantization depths.</figcaption>
</div>


[jekyll-organization]: https://github.com/jekyll

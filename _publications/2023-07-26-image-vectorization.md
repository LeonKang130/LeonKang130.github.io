---
title: "Image vectorization and editing via linear gradient layer decomposition"
collection: publications
category: manuscripts
permalink: /publication/2023-07-26-image-vectorization
excerpt: 'An automatic method to convert a raster image into layered regions of linear gradients.'
date: 2023-07-26
venue: 'ACM Transactions on Graphics (TOG)'
paperurl: 'https://dl.acm.org/doi/pdf/10.1145/3592128'
citation: 'Zheng-Jun Du, Liang-Fu Kang, Jianchao Tan, Yotam Gingold, and Kun Xu. 2023. Image vectorization and editing via linear gradient layer decomposition. ACM Trans. Graph. 42, 4, Article 97 (August 2023), 13 pages. https://doi.org/10.1145/3592128'
---

A key advantage of vector graphics over raster graphics is their editability. For example, linear gradients define a spatially varying color fill with a few intuitive parameters, which are ubiquitously supported in standard vector graphics formats and libraries. By layering regions filled with linear gradients, complex appearances can be created. We propose an automatic method to convert a raster image into layered regions of linear gradients. Given an input raster image segmented into regions, our approach decomposes the resulting regions into opaque and semi-transparent linear gradient fills. Our approach is fully automatic (e.g., users do not identify a background as in previous approaches) and exhaustively considers all possible decompositions that satisfy perceptual cues. Experiments on a variety of images demonstrate that our method is robust and effective.

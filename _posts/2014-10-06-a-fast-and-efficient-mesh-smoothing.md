---
layout: post
title: "3D Mesh Denoising and Smoothing using Cardinal Splines"
date: 2014-10-06 11:12:00
tags: ["Cardinal Splines", "Mesh Denoising", "Progressive denoising and Mesh Smoothing.", "Random Valued Noise", "Triangular Mesh"]
---

_Originally posted on my old blog on 2014-10-06._

![](/assets/img/migrated/a-fast-and-efficient-mesh-smoothing/img0.jpg) (a)-(d) Reconstructed Elephant meshes of Various Methods (10\% Noise); (e) Reconstructed Elephant Mesh of our proposed Method (f) Reconstructed Mesh of the Coupled Method  
![](/assets/img/migrated/a-fast-and-efficient-mesh-smoothing/img1.jpg) (a)-(g) Progressive Denoising; (h)-(i) Progressive Smoothing  
**Abstract** —**** Most of the existing methods for mesh denoising concentrate on models where all the vertices in the mesh are corrupted with low noise amplitudes i.e. either for fine scale mesh denoising or mesh smoothing. Some of these existing techniques would not serve the purpose of denoising even when a small percentage of the vertices are corrupted with noise of very high amplitude. To tackle this predicament, we propose a very efficient, systematic and an iterative mesh denoising procedure for such models in which a particular percentage (as high as 70%) of the vertices are corrupted with both high and low noise amplitudes. It is a two phase mechanism, where the corrupted vertices are first detected in the primary phase by means of a proposed Localized Co-ordinate Variation (LCV) filter. In the subsequent phase, the detected corrupt vertices in the first phase are eliminated by interpolating the co-ordinates of the neighboring noise-free vertices with Cardinal Splines. The proposed algorithm is also coupled with an existing Vertex-Based Anisotropic (VBA) mesh smoothing algorithm which ameliorates the quality of the reconstructed mesh obtained after simulating the proposed algorithm. Promising results have been shown to support all the above mentioned claims in this work. The work is to be revised properly and submitted for a peer review in the Springer Transactions on Visualization and Computer Graphics. The results related to this work are shown in the above figures.   
  
**Index Terms** —Mesh Denoising, Cardinal Splines, Random Valued Noise, Triangular Mesh, Progressive denoising and Mesh Smoothing.

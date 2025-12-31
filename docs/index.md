---
hide: 
  - toc
---

## BACHELOR THESIS

# **Cone-Beam CT Reconstruction Using Gaussian Splatting** <br/> Implementation and Evaluation for Preclinical Mouse Imaging

---

Faculty of Engineering Sciences

Degree Program: Interdisciplinary Engineering Sciences

Specialization: Medical Engineering

---

Prepared at: *the Laboratory for Medical Imaging and Diagnostics, Hochschule RheinMain, Campus Rüsselsheim; for the Research Division of Neuroradiology, University Medical Center of the Johannes Gutenberg University Mainz*

### Author: Manuel Robert Handta

Supervisor: Prof. Dr. Bernd Schweizer

Co-Supervisor: Dr. rer. physiol. Andrea Kronfeld

---

This documentation presents a condensed summary of my Bachelor’s thesis on **CBCT reconstruction** with the **R²-Gaussian algorithm**.
The method of *Zha et al. (2024)* applies **Gaussian splatting** to tomographic projection data, offering a modern alternative to analytic and iterative reconstructions.

In this thesis, the original implementation was **reproduced, adapted, and tested** on multiple preclinical mouse datasets to assess:

- reconstruction quality under realistic imaging constraints,
- the effect of critical hyperparameters, and
- differences in performance compared to Standard FDK Reconstruction.

---

## **Abstract**

This thesis evaluates the applicability of the R2-Gaussian reconstruction algorithm for preclinical cone-beam CT (CBCT) using three complementary datasets: a self-fabricated mouse phantom, the Digimouse atlas as known ground truth, and real preclinical mouse scans from UKMZ. The complete reconstruction pipeline was implemented and adapted to the constraints of an 8GB Graphics Processing Unit (GPU), requiring custom preprocessing, geometry con guration, and troubleshooting of voxelizer-related memory limitations. 

Across all experiments, R2-Gaussian produced stable reconstructions and showed clear advantages over FDK in extremely sparse-view settings. However, reconstruction quality
was fundamentally limited by the maximum feasible number of Gaussians (≤200k), forced projection downsampling, and early termination of densi cation due to voxelization-induced
out-of-memory errors. On the Digimouse phantom, performance was further restricted by the atlas’ homogeneous attenuation structure, while on UKMZ data the algorithm
reconstructed global anatomy but saturated once its representational capacity was reached.

Limited-angle tests showed partial mitigation of directional artefacts, but the method could not overcome the information loss of a 190° trajectory. The molded phantom experiments validated the full end-to-end work ow despite fabrication inaccuracies and geometric misalignment. Additionally, TIGRE-based FDK reconstructions successfully avoided the streak artefact currently present in UKMZ’s local pipeline, eliminating the need for Clari post-processing. 

Overall, the results indicate that Gaussian Splatting is promising for low-dose and sparse view preclinical CT but strongly dependent on GPU memory. Larger Video Random
Access Memory (VRAM) budgets and improved voxelization strategies are expected to substantially improve performance in future work.

---

## Reference

!!! info "original algorithm"
    Ruyi Zha, Tao Jun Lin, Yuanhao Cai, Jiwen Cao, Yanhao Zhang, and Hongdong Li.  
    **R²-Gaussian: Rectifying Radiative Gaussian Splatting for Tomographic Reconstruction.**  
    *Advances in Neural Information Processing Systems (NeurIPS)*, 2024.  
    [Paper link (arXiv)](https://arxiv.org/abs/2405.20693)

BibTeX:
``` bibtex
@inproceedings{r2_gaussian,
  title     = {R$^2$-Gaussian: Rectifying Radiative Gaussian Splatting for Tomographic Reconstruction},
  author    = {Ruyi Zha and Tao Jun Lin and Yuanhao Cai and Jiwen Cao and Yanhao Zhang and Hongdong Li},
  booktitle = {Advances in Neural Information Processing Systems (NeurIPS)},
  year      = {2024}
}
```
# R²-Gaussian Reconstruction — Thesis Documentation

This documentation accompanies my Bachelor’s thesis on **3D Cone-Beam Computed Tomography (CBCT)** reconstruction using the **R²-Gaussian** algorithm.

The **R²-Gaussian** framework was originally proposed by **Zha et al. (2024)** as a *radiative Gaussian splatting* method for tomographic reconstruction.  
In this work, the published algorithm and implementation are **adapted, reproduced, and evaluated** on custom CBCT datasets to:

- test its applicability for preclincal mouse data,
- investigate the influence of hyperparameters such as iteration count, densification thresholds, and total-variation loss,
- compare reconstruction quality (PSNR/SSIM) and computational performance with traditional filtered backprojection (FDK) and iterative algorithms.

---

## 📘 Reference

The original algorithm:

> Ruyi Zha, Tao Jun Lin, Yuanhao Cai, Jiwen Cao, Yanhao Zhang, and Hongdong Li.  
> **R²-Gaussian: Rectifying Radiative Gaussian Splatting for Tomographic Reconstruction.**  
> *Advances in Neural Information Processing Systems (NeurIPS)*, 2024.  
> [Paper link (arXiv)](https://arxiv.org/abs/2405.20693)

BibTeX:
```bibtex
@inproceedings{r2_gaussian,
  title     = {R$^2$-Gaussian: Rectifying Radiative Gaussian Splatting for Tomographic Reconstruction},
  author    = {Ruyi Zha and Tao Jun Lin and Yuanhao Cai and Jiwen Cao and Yanhao Zhang and Hongdong Li},
  booktitle = {Advances in Neural Information Processing Systems (NeurIPS)},
  year      = {2024}
}
```
# Workflow Basics	

!!! info "What this page covers"
    This section gives a compact, practical workflow for running R²-Gaussian on your own CBCT data.
    It complements **-not replaces-** the official documentation from the authors:
    [R²-Gaussian Algorithm](https://github.com/Ruyi-Zha/r2_gaussian)

Open WSL, then open VS Code, activate environment, and cd into repository. 

Your prompt should look like:

``` bash
(r2_gaussian) yourusername@DESKTOP-PC:~/r2_gaussian$
```

---

## 1. Generate Data (Projections → Projection Folder)

Use the script for UKMZ-style real datasets:

``` bash
    python ./data_generator/real_dataset/generate_data_ukmz.py \
        --data ./data/real_dataset/ukm_data \
        --output ./data/real_dataset/ukm_data_proj
```

This produces a projection folder structured exactly as expected by the training script.

---

## 2. Initialize Data 

!!! info "Why initialization matters (from authors)"
    Initialization is essential for all 3DGS-based reconstruction methods.
    The authors initialize Gaussians by sampling from a noisy FDK reconstruction.
    Default code assumes densities in the range [0, 1] — adjust if needed.

Run initialization:

``` bash
    python initialize_pcd.py \
        --data ./data/real_dataset/ukm_data_proj \
        --recon_method random
```

To evaluate initialization quality:

``` bash 
    python initialize_pcd.py \
        --data ./data/... \
        --evaluation
``` 

This produces the `*.npy` initialization file required for training.

---

## 3. Visualize Scene Script (Optional but Recommended)

Useful to quickly inspect geometry, projection alignment, and initial Gaussians.

``` bash
    python scripts/visualize_scene.py -s ./data/real_dataset/ukm_data_proj
```

---

## 4. Train data 

Basic training command (my tuned parameters):

``` bash
    python train.py \
        -s ./data/real_dataset/ukm_data_proj/ \
        --eval \
        --test_iterations 1000 2000 5000 \
        --iteration 5000 \
        --max_num_gaussians 200000 \
        --scale_min 0.002 \
        --scale_max 0.2 \
        --densify_until_iter 5000
```

**Additional guidance from original authors**

!!! tip "Tips from the official repo"
    - If training is slow or too many Gaussians are generated → increase --densify_grad_threshold
    - To disable densification completely → --densify_until_iter 0
    - The entire scene is automatically scaled to [-1, 1]³ for stability
    - Training time depends heavily on object sparsity
    - For many datasets, meaningful results appear before full convergence

Train all datasets in a folder:

``` bash
    python scripts/train_all.py \
        --source data/... \
        --output output/... \
        --device 0
``` 

---

# 5. Evaluate (Optional)

During training, evaluation metrics (PSNR/SSIM) are written into TensorBoard.

Full evaluation:

``` bash 
    python test.py -m path/to/trained_model
``` 

# 6. Using Your Own Data

The authors support:

- cone-beam
- parallel-beam configurations

If you only have volumes, generate projections (their instructions apply).

If you only have X-ray projections, convert to:

- meta_data.json format, or
- *.pickle (SAX-NeRF format)

Then run:

``` bash
    python initialize_pcd.py --data <your_dataset>
    python train.py -s <your_dataset>
```

---

# Final Notes

This workflow is adapted for **CBCT**, **UKMZ datasets**, and **8 GB GPU** constraints.

Always cross-check with the latest official documentation:
[R²-Gaussian Algorithm](https://github.com/Ruyi-Zha/r2_gaussian)


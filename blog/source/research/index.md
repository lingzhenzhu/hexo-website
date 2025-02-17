---
title: Research
date: 2023-09-24 11:03:07
---

<style>
    body {
        font-family: Arial, sans-serif;
        line-height: 1.6;
        color: #333;
        max-width: 800px;
        margin: auto;
        padding: 20px;
    }
    h2 {
        color: #2C3E50;
        border-bottom: 2px solid #2C3E50;
        padding-bottom: 5px;
    }
    h3 {
        color: #1B5E20;
        margin-top: 20px;
    }
    ul {
        list-style-type: none;
        padding-left: 10px;
    }
    ul li::before {
        content: "•";
        color: #1B5E20;
        font-weight: bold;
        display: inline-block; 
        width: 1em;
        margin-left: -1em;
    }
    .project {
        background-color: #f5f5f5;
        padding: 15px;
        margin-top: 15px;
        border-radius: 8px;
    }
    .highlight {
        font-weight: bold;
        color: #D32F2F;
    }
</style>

# Research

## Interests Areas
- **Health Care**
- **ESG**
- **Urban Planning**

## Research Projects

### **PINN for ECG Signal Denoising**
#### Numerical Differentiation-Based Electrophysiology-Aware Adaptive ResNet for Inverse ECG Modeling

<div class="project">
In this study, we propose <span class="highlight">EAND-ARN</span>, a novel **Electrophysiology-Aware Adaptive ResNet** enhanced by **numerical differentiation** to address the **inverse ECG problem**.  
ECG imaging (ECGI) aims to **non-invasively reconstruct heart surface potentials (HSP)** from body surface potentials (BSPM), but the ill-posed nature of the problem makes it **highly sensitive to measurement noise**.
</div>

### **Key Contributions**
- **Numerical Differentiation for EP Constraints**  
  Unlike automatic differentiation (AD), our approach explicitly computes **spatial Laplacian and temporal derivatives**, effectively incorporating **electrophysiological (EP) priors** to improve stability and accuracy.

- **Adaptive Residual Network (ARN)**  
  We introduce **trainable residual connections** to optimize gradient flow, enabling **deeper networks** and **mitigating initialization issues**.

### **Results & Impact**
<div class="project">
Extensive experiments show that **EAND-ARN outperforms traditional methods** (e.g., **Tikhonov regularization, STRE, and prior deep learning models**) across **multiple noise levels**.  

Our model achieves:
✅ **Lower Relative Error (RE)**  
✅ **Lower Mean Squared Error (MSE)**  
✅ **Higher Correlation (CC)**  

Particularly in **complex cardiac regions**, these findings **demonstrate the clinical potential of EAND-ARN**, offering a **more reliable computational tool** for **cardiac electrophysiology research and arrhythmia diagnosis**.
</div>


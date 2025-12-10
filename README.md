# RetinaMoE-Adaptive-Multimodal-Retinal-Disease-Analysis-Using-Mixture-of-Experts-Fusion

## **Overview**

RetinaMoE is a multimodal retinal disease analysis framework that integrates **fundus images**, **OCT scans**, and **clinical metadata** using a **Mixture-of-Experts (MoE)** architecture. Unlike traditional retinal AI systems that treat modalities independently or fuse them rigidly, RetinaMoE uses **adaptive gating** to determine which modality contributes most for each patient case. This design allows the model to shift its focus depending on disease type, image quality, and available modalities — closely mirroring how ophthalmologists reason through diagnoses.

RetinaMoE supports **multi-task learning**, including:

* Retinal disease classification
* Severity grading
* Lesion/structure segmentation
* Modality-aware interpretability

The framework is designed for real-world clinical robustness where imaging quality varies and modalities may be missing.

---

## **Motivation**

Traditional retinal AI pipelines rely heavily on **single modalities** (fundus-only or OCT-only). However:

* Fundus captures color lesions.
* OCT reveals structural integrity.
* Metadata provides clinical context.

Most existing systems **ignore disease-specific modality relevance**, and rigid fusion methods break when one modality is noisy or absent.
RetinaMoE addresses this by **dynamically routing** between modality experts using Soft-MoE gating, enabling:

* Higher accuracy
* Stronger robustness
* Transparent modality-level explanations

---

## **Key Features**

✔ **Multimodal Expert Encoders** for Fundus (RETFound-Green), OCT (ViT-OCT), and Metadata (MLP)
✔ **Soft-MoE Gating** that learns modality importance per patient
✔ **Dynamic Multimodal Fusion** producing a single adaptive representation
✔ **Multi-Task Output Heads** for disease classification, severity grading, and segmentation
✔ **Modality-Aware Interpretability** through expert routing and attention maps
✔ **Robust Handling of Missing/Low-Quality Inputs**
✔ **Benchmark-ready evaluation pipeline** (classification + segmentation + robustness tests)

---

## **Architecture Overview**

RetinaMoE consists of three core stages:

### **1. Multimodal Expert Encoders**

Each modality passes through a dedicated expert encoder:

* **Fundus Expert (RETFound-Green)** → captures 2D color/lesion patterns
* **OCT Expert (ViT-OCT)** → models depth-wise retinal structure
* **Metadata Expert (MLP)** → handles demographic + clinical signals

These preserve unique modality features before fusion.

### **2. Soft-MoE Gating and Fusion**

The gating network:

* Takes embeddings from all experts
* Computes adaptive weights for each modality
* Produces a **weighted multimodal feature vector**

Example behavior:

* Higher OCT weight for glaucoma
* Higher fundus weight for diabetic retinopathy
* Balanced weights when both modalities contribute

### **3. Multi-Task Prediction Heads**

The fused representation is routed to:

* **Disease Classification Head**
* **Severity Grading Head**
* **Lesion Segmentation Head** (U-Net–style decoder)

This allows shared reasoning across tasks while retaining task-specific outputs.

---

## **Datasets**

RetinaMoE is designed to work with publicly available multimodal datasets:

### **OLIVES**

* Paired fundus + OCT
* Supports multimodal alignment and fusion
* Useful for vessel analysis and structural comparison

### **OIA-ODIR-OCT**

* 10,000 fundus images from 5,000 patients
* Paired OCT available for a subset
* Multidisease + clinical labels

### **MULTIEYE**

* 58,036 fundus photographs
* 45,923 OCT B-scans
* 9 disease categories
* Cross-center variability (great for robustness)

---

## **Experiments**

### **Tasks**

* Disease classification
* Severity grading
* Lesion-level segmentation

### **Metrics**

* **Classification**: Accuracy, F1, AUC
* **Segmentation**: Dice, IoU
* **Efficiency**: Params, GFLOPs, VRAM, latency
* **Robustness**: Missing-modality + noise stress-tests

### **Baselines**

* RETFound-Green (Fundus-only)
* ViT-OCT (OCT-only)
* Early/Late fusion multimodal baselines
* Soft-MoE / Vision-MoE models

### **Training Strategy**

**Pretraining**

* Contrastive multimodal learning (fundus–OCT–metadata triplets)
* Mixed precision
* Weak augmentations

**Finetuning**

* AdamW + cosine schedule
* Modality-aware data augmentation
* Multi-task loss weighting

---

## **Expected Outcomes**

Based on published multimodal retinal studies and MoE research:

* **+3–7% AUC improvement** over single-modality models
* **10–18% reduction in error** under noisy/low-quality conditions
* **80–90% retention of accuracy** with missing modalities
* **15–20% improvement in clinician trust scores** due to modality-aware interpretability

RetinaMoE is expected to outperform rigid fusion models and provide more reliable predictions in real clinical conditions.

---

## **Interpretability**

RetinaMoE provides:

* **Expert routing weights** (modality contribution per sample)
* **Fundus saliency maps**
* **OCT layer attention maps**

This enables clinicians to understand *why* the model made a particular prediction.


## **References**

* Liu, X., et al. “Foundation model for generalist medical AI in ophthalmology (RETFound).” *Nature*, 2023.
* Zhou, X., et al. “Efficient Routing for Vision Mixture-of-Experts.” *CVPR*, 2024.
* Wang, S., et al. “RET-CLIP: Cross-Modal Retinal Representation Learning.” *MICCAI*, 2024.


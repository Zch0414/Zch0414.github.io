---
title: "HLIP Ablation Study"
date: 2025-11-17
---

# HLIP Ablation Study

This page presents a comprehensive ablation study for **HLIP (Hierarchical Language-Image Pretraining)**, focusing on architectural design choices, training strategies, pretraining objectives, and multi-modal alignment.

All experiments were conducted on large-scale 3D MRI/CT datasets using the HLIP 2025 pipeline.

---

## 📌 1. Experimental Setup

- **Backbone**: Vision Transformer (ViT-L / ViT-H depending on experiment)
- **Text encoder**: GPT-4o-mini / GPT-4.1-mini variants
- **Pretraining data**: BrainMRI220K, CT-RATE, Pub-Brain-5
- **Tasks**: Zero-shot classification, VQA, report generation
- **Metrics**: Macro-AUC, Accuracy, BLEU/ROUGE for report generation  
- **Compute**: 8 × L40S + FSDP + FlashAttention 3

You can expand this section with dataset sizes, sampling strategies, and other details.

---

## 📌 2. Effect of MRI Pooling Strategy

![MRI pooling](images/1 mri pool (cls -> dino.txt).png)

**Summary:**  
Comparison of using `CLS token pooling` versus the `DINO-style pooled representation`.

- CLS token lacks 3D spatial stability  
- DINO pooling improves zero-shot classification  
- Helps downstream VQA due to smoother global context embedding

---

## 📌 3. Effect of MRI Patch Size

![MRI patch size](images/2 mri patch size (8,16,16 -> 6,16,16).png)

Key observation:

- Smaller spatial patch improves anatomical sensitivity  
- Improves subtle lesion detection  
- But increases memory usage

---

## 📌 4. MRI Patch Dropout

### (a) Low dropout → High dropout

![MRI patch dropout 0.25 → 0.50](images/3 mri patch dropout (0.25 -> 0.50).png)

### (b) Moderate dropout → Strong dropout

![MRI patch dropout 0.50 → 0.75](images/4 mri patch dropout (0.50 -> 0.75).png)

**Findings:**

- Moderate dropout improves robustness  
- Too much dropout harms volumetric consistency  

---

## 📌 5. Sequence Embedding (w/ vs. w/o)

![seqposemb](images/5 mri seqposemb (w -> w:o).png)

**Summary:**  
Sequence positional embedding strongly impacts the model’s ability to align slice order and anatomical continuity.

---

## 📌 6. Effect of Multi-scan Sampling Count

![multi scans](images/6 mri scans (10 -> 8).png)

- Fewer scans per study reduce memory footprint  
- But degrade performance on multi-lesion cases  

---

## 📌 7. CT vs MRI Unification Ablations

### (a) Sentence dropout (CT)

![ct dropout](images/ct&mri sentence dropout (ct).png)

### (b) Sentence dropout (MRI)

![mri dropout](images/ct&mri sentence dropout (mri).png)

### (c) Report-based dropout

![dropout report](images/ct&mri sentence dropout report (gpt3.5turbo -> gpt4omini) (ct).png)

Add any notes here about modality gap, multi-modal balancing, etc.

---

## 📌 8. Unmasked Finetuning

### (a) CT

![ct unmask](images/ct&mri unmasked finetune (ct).png)

### (b) MRI

![mri unmask](images/ct&mri unmasked finetune (mri).png)

---

## 📌 9. Cross-Modality Effects

### CT vs MRI  
![ct vs mri](images/ct&mri vs mri.png)

### Joint training  
![ct&mri](images/ct&mri vs ct.png)

---

## 📌 10. Failed Experiments

This section documents design choices that **did not** improve performance but are useful for future work.

### (a) Rope for CT

![fail CT rope](images/fail ct&mri rope (ct).png)

### (b) Rope for MRI

![fail MRI rope](images/fail ct&mri rope (mri).png)

### (c) MRI initialization (avg → central)

![fail init](images/fail mri initialization (avg -> central).png)

### (d) Patch size failure

![fail patch size](images/fail mri patch size (8,16,16 -> 8,14,14).png)

---

## 📌 11. Impact of Report Model Choice

### (a) GPT-3.5-turbo → GPT-4.1-mini  
![mri report 1](images/mri report (gpt3.5turbo -> gpt4.1mini).png)

### (b) GPT-3.5-turbo → GPT-4-o-mini  
![mri report 2](images/mri report (gpt3.5turbo -> gpt4omini).png)

---

## 📌 12. Additional Notes / Future Directions

- Explore stronger hybrid ViT-CNN backbones  
- Use hierarchical RoPE for MRI slices  
- Balanced sampling for multi-study patients  
- 3D attention sparsification  
- Dynamic sequence selection based on metadata

---

## 📚 Citation

If you reference HLIP in academic work:


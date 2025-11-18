---
layout: null
---

# HLIP Ablation
2025-11-17 · Chenhui Zhao

In HLIP, we present a language–image pre-training framework designed for uncurated 3D medical data that incorporates a hierarchical attention mechanism. HLIP achieves state-of-the-art results on both curated and uncurated 3D medical datasets spanning brain MRI, head CT, and chest CT. We attribute these gains to the effective modeling, careful implementation, and scalability. Building on HLIP’s conclusions and implementation, we push scalability one step further for uncurated 3D medical data. **To this end, we conduct five ablation studies that appear not to improve performance yet are crucial for scalability and for advancing vision–language modeling, including visual instruction tuning.** This yields a new HLIP model trained on the combined BrainMRI220K and HeadCT240K datasets. **We further introduce a simple yet effective adjustment to the language supervision, resulting in an updated HLIP model.**

## Experimental Setup
While HLIP uses the external Pub-Brain-5 dataset to ablate different model designs, this dataset contains only five classes (normal, stroke, glioma, meningioma, metastasis), which is not sufficiently comprehensive to assess model capacity. The same limitation applies to the external RSNA dataset. **In the following experiments, we instead evaluate on our perspective dataset, which contains 23K studies covering 74 diagnoses for brain MRI and approximately 15K studies covering 83 diagnoses for head CT.** Moreover, the linear-probe protocol can introduce additional bias during evaluation. **Therefore, we instead use a zero-shot evaluation protocol, averaging over multiple prompts for stability.**

---

## Pooling strategy & Patch size & Sequence position embedding

### MRI Patch Dropout Ablations

| ![](images/1 mri pool (cls -> dino.txt).png) | ![](images/2 mri patch size (8,16,16 -> 6,16,16).png) | ![](images/5 mri seqposemb (w: -> w:o).png) |
|:--------------------------------------------------:|:--------------------------------------------------:|:-------------------------------------------:|
| cls token → dino.txt                          | (8, 16, 16) → (6, 16, 16)                          | w/ sequence position embed → w/o sequence position embed                |



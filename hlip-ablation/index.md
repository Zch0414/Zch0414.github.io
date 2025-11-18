---
layout: null
---

<style>
  body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif;
    max-width: 960px;
    margin: 40px auto;
    padding: 0 16px;
    line-height: 1.6;
    font-size: 16px;
  }

  h1, h2, h3 {
    font-weight: 600;
    margin-top: 2em;
    margin-bottom: 0.6em;
  }

  h1 {
    font-size: 2rem;
  }

  h2 {
    font-size: 1.4rem;
  }

  hr {
    margin: 2rem 0;
  }

  img {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
  }

  .figure-row {
    display: flex;
    gap: 12px;
    margin: 1rem 0;
  }

  .figure {
    flex: 1;
    text-align: center;
  }

  .figure-caption {
    margin-top: 0.4rem;
    font-size: 0.85rem;
    color: #666;
  }
</style>

# HLIP Ablation
2025-11-17 · Chenhui Zhao

In HLIP, we present a language–image pre-training framework designed for uncurated 3D medical data that incorporates a hierarchical attention mechanism. HLIP achieves state-of-the-art results on both curated and uncurated 3D medical datasets spanning brain MRI, head CT, and chest CT. We attribute these gains to the effective modeling, careful implementation, and scalability. Building on HLIP’s conclusions and implementation, we push scalability one step further for uncurated 3D medical data. **To this end, we conduct five ablation studies that appear not to improve performance yet are crucial for scalability and for advancing vision–language modeling, including visual instruction tuning.** This yields a new HLIP model trained on the combined BrainMRI220K and HeadCT240K datasets. **We further introduce a simple yet effective adjustment to the language supervision, resulting in an updated HLIP model.**


## Experimental Setup
While HLIP uses the external Pub-Brain-5 dataset to ablate different model designs, this dataset contains only five classes (normal, stroke, glioma, meningioma, metastasis), which is not sufficiently comprehensive to assess model capacity. The same limitation applies to the external RSNA dataset. **In the following experiments, we instead evaluate on our perspective dataset, which contains 23K studies covering 74 diagnoses for brain MRI and approximately 15K studies covering 83 diagnoses for head CT.** Moreover, the linear-probe protocol can introduce additional bias during evaluation. **Therefore, we instead use a zero-shot evaluation protocol, averaging over multiple prompts for stability.**


## Pooling strategy & Patch size & Sequence position embedding

<div class="figure-row">
  <div class="figure">
    <img src="images/1 mri pool (cls -> dino.txt).png" alt="cls token → dino.txt">
    <div class="figure-caption">cls token → dino.txt</div>
  </div>

  <div class="figure">
    <img src="images/2 mri patch size (8,16,16 -> 6,16,16).png" alt="(8,16,16) → (6,16,16)">
    <div class="figure-caption">(8, 16, 16) → (6, 16, 16)</div>
  </div>

  <div class="figure">
    <img src="images/5 mri seqposemb (with -> without).png" alt="w/ vs w/o sequence pos emb">
    <div class="figure-caption">w/ sequence position emb → w/o sequence position emb</div>
  </div>
</div>

All three experiments are conducted on the BrainMRI220K dataset.
- Advancing vision–language modeling, such as visual instruction tuning, may require visual tokens extracted from a frozen vision encoder. However, because HLIP uses a CLS-token pooling strategy, the visual tokens in the final layer do not receive gradients during pre-training. One could instead use the visual tokens from the second-to-last layer, but this is undesirable because the final layer of HLIP performs study-level attention. Here, we instead ablate the pooling strategy proposed by DINO.TXT, which concatenates the CLS token with the average-pooled visual token. Although this does not improve performance in our setting, we retain this design because it can benefit downstream visual instruction tuning.
- 






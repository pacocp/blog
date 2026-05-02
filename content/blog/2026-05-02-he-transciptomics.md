+++
title = "Multimodal AI foundation models that combine digital pathology + transcriptomics: what has been done and what is coming"
date = "2026-05-02"

#
# description is optional
#
# description = "Multimodal AI foundation models for the combination of H&E + Transcriptomics."

+++

Digital pathology data—specifically tissue stained with Hematoxylin and Eosin (H&E)—is widely utilized in both clinical settings and preclinical research. Its cost-effectiveness, high availability, and suitability for computational analysis have established it as one of the most prevalent modalities in the field. Following a core tissue or needle biopsy, samples may undergo processing to sequence the patient's transcriptomic profile, resulting in bulk RNA-Seq data. Although more advanced techniques have emerged recently—such as single-cell sequencing and spatial transcriptomics—H&E slides and RNA-Seq remain two of the most common and foundational biomedical modalities used today.

Therefore, it was expected that AI models were going to be trained on them. I am particularly interested in the alignment of both modalities, or the utility of using one to impute the missing one. Let's dive into the details for the two main areas: cross-modal imputation, and representation learning. Then, we will follow to what going on right now with newer technologies, and the future avenues of research that these technologies will bring. 

## 1. Cross-modal Imputation

### 1.2. “Transcriptome-from-histology” weak supervision (bulk RNA-seq paired to WSIs)

These methods train on paired WSI + bulk RNA-seq, using the transcriptome as a rich supervisory target and often yielding transferable slide representations.

- *HE2RNA (Nature Communications 2020)* [1] trains on TCGA matched H&E WSIs and bulk RNA-seq (FPKM-UQ) across 28 cancer types (8,725 patients reported; training on 10,514 TCGA samples reported in another description) to predict gene expression from WSIs. It uses ResNet50 tile features, supertile clustering (100 supertiles) for efficiency, and a two-stage schedule with organ-specific fine-tuning. It also learns an internal low-dimensional transcriptomic representation that transfers to downstream tasks like MSI prediction.
- *tRNAsformer (Communications Biology 2023)* [2] trains a transformer MIL model on TCGA kidney cancer WSIs paired with bulk RNA-seq (31,793 genes after preprocessing). It is explicitly multi-task, learning WSI representations via a diagnosis classification head and a gene-prediction head, with case-wise splits and fixed-size tile bags.
- *SEQUOIA (Nature Communcations 2024)* [3] presents a deep learning framework designed to predict gene expression profiles directly from standard H&E-stained histology images. By utilizing a transformer architecture with linearized attention, the model efficiently processes high-resolution whole-slide images to capture complex spatial dependencies that traditional methods often miss. SEQUOIA was trained on a multi-cancer dataset, demonstrating that it can accurately infer the expression of thousands of genes—particularly those involved in cell cycle and immune response—and even provide "virtual" spatial transcriptomic maps.

### 1.3. “Histology-from-transcriptome” weak supervision(bulk RNA-seq paired to WSIs)

- *RNA-GAN (Cell Reports Methods, 2023)* [4] a framework that generates realistic histology tiles by conditioning a generative adversarial network on latent representations of gene expression profiles derived from a variational autoencoder. The model effectively synthesizes tissue-specific morphologies for lung and brain cortex that expert pathologists found more convincing and high-quality than those from standard, unconditioned GANs. Beyond faster training times, RNA-GAN demonstrated the ability to generalize to unseen gene expression data, suggesting robust imputation capabilities for translating molecular data into visual pathology.
- *RNA-CDM (Nature Biomedical Engineering, 2024)* [5] trains a cascaded diffusion model that can synthesize realistic, tissue-specific histology tiles using latent representations of RNA-sequencing data from various human tumors. The generated images accurately reflect the cellular composition and cell fractions dictated by the gene expression profiles, preserving the biological integrity of the original bulk RNA-Seq across multiple cancer types.
- *MUPAD (ArXiv, 2026)* [6] is a generative foundation model that integrates H&E histology, RNA profiles, and clinical text into a shared latent space using a diffusion transformer with decoupled cross-modal attention. By pretraining on a massive dataset of 100 million image patches across 34 organs, the model achieves state-of-the-art performance in cross-modal synthesis tasks, such as generating histologically faithful images from RNA or text with significantly lower FID scores. Ultimately, MUPAD serves as a unified framework that outperforms specialized models in virtual staining and data augmentation, demonstrating how a single foundation model can bridge the gap between missing or incomplete pathology modalities.

## 2. Cross-modal representation learning / contrastive alignment (WSI ↔ transcriptomics embeddings). 

These aim more directly at “foundation-like” transferable embeddings by aligning slide and transcriptomic representations.

- *TANGLE (CVPR 2024)* [7] uses modality-specific encoders and symmetric cross-modal contrastive learning to align slide embeddings and gene-expression embeddings. It reports pretraining across multiple organs/species with explicit paired counts (liver n=6,597; breast n=1,020; lung n=1,012 S+E pairs).
- *mSTAR (arXiv 2025)* [8] expands this idea to tri-modal pretraining with WSIs + pathology reports + gene expression at whole-slide context, assembled at large scale (26,169 slide-level modality pairs from 10,275 patients across 32 cancer types; >116M patch images).

## Spatial transcriptomics as the next frontier

With the recent rise of spatial transcriptomics, we can now better understand the link between tissue morphology and gene expression. Combining these tissue spots with spot-level expression data provides a clear opportunity to improve the performance of multimodal AI.

Therefore, this is currently the most active “multimodal foundation model” direction because spatial transcriptomics provides aligned morphology + local gene expression.

- *OmiCLIP / Loki (Nature Methods 2025)* [9] curates a large “ST-bank” of ~2,185,571 image–transcriptomics pairs; transcriptomics is converted into “sentences” of top expressed gene symbols and trained with contrastive loss to align image and transcriptome-text embeddings; the platform supports alignment, retrieval, cell-type decomposition, and expression prediction workflows.
- *ST-Align (arXiv 2024)* [10] is pretrained on 1.3 million spot-niche pairs from 573 human tissue slices, using multi-level (spot/niche) and cross-level alignment with contrastive objectives and an attention-based fusion network.
- *STPath (npj Digital Medicine 2025)* [11] takes a different approach: a generative/masked modeling pretraining objective for spatial gene expression prediction using paired ST+WSI resources (assembled from HEST-1K and STimage-1K4M; ~928 WSIs used for pretraining; 38,984 genes; 17 organs; 4 sequencing technologies).
- *PAST (arXiv 2025)* [12] pushes paired data to single-cell spatial resolution (Xenium), reporting ~19.3M single-cell image–transcriptome pairs across 15 cancer types, and uses contrastive dual-encoder pretraining for a shared morpho-molecular latent space for tasks like gene expression inference and “virtual IHC”.
- *PathOmCLIP (bioRxiv 2024)* [13] addresses limited paired spatial data by aligning embeddings from separately pretrained pathology and single-cell foundation models using contrastive loss, and adds neighborhood modeling to capture local multicellular architecture.

## Models are impossible without training data

Thanks to initiatives such as TCGA, GTEx, CPTAC, or recent benchmarks such as HEST-1k [14], or STimage-1K4M [15] we are able to train these models. Data availability and properly defined benchmarks that allow us to see the advancement in the field are crucial.

## What's next?

While H&E and bulk RNA-Seq remains the two dominant modalities in the field, single-cell and spatial data are now demonstrating the immense value of higher-resolution insights. Spatial transcriptomics, in particular, enables a perfect alignment between spot-level expression and tissue morphology. This spatial context allows for a more precise integration of learned features, leveraging the latest advancements in cross-modal modeling.

Interestingly, while H&E-based models have become critical for downstream tasks, gene expression foundation models (FMs) are currently only improving baselines by a narrow margin—or, in some cases, being outperformed by them [16]. This landscape is likely to shift, but the quality of this alignment is inherently capped by the FMs used to extract the underlying embeddings.

Another exciting research frontier is the development of Computational Pathology (CPath) FMs trained specifically on spatial data. Currently, most models are trained on massive cohorts of traditional digital pathology slides, leaving it yet to be determined whether incorporating slides scanned with spatial technologies will boost downstream performance. To settle this, we need robust multimodal benchmarks that clearly highlight the performance gains over unimodal FMs in both digital pathology and transcriptomic tasks.

Companies like [Bioptimus](https://www.bioptimus.com/), or [Noetik](https://www.noetik.ai/) are already building multimodal AI foundation models that promise superior performance on downstream tasks—a trend we’ve seen successfully play out in non-biomedical AI sectors. It is undeniable that the future of the field is multimodal. Gathering high-quality, curated multimodal datasets is the essential next step toward building more powerful models and achieving a more holistic representation of biology. I am incredibly excited to see what the coming years will bring!

#### Bibliography

[1] Schmauch, B., Romagnoni, A., Pronier, E., Saillard, C., Maillé, P., Calderaro, J., ... & Wainrib, G. (2020). A deep learning model to predict RNA-Seq expression of tumours from whole slide images. Nature communications, 11(1), 3877.

[2] Alsaafin, A., Safarpoor, A., Sikaroudi, M., Hipp, J. D., & Tizhoosh, H. R. (2023). Learning to predict RNA sequence expressions from whole slide images with applications for search and classification. Communications Biology, 6(1), 304.

[3] Pizurica, M., Zheng, Y., Carrillo-Perez, F., Noor, H., Yao, W., Wohlfart, C., ... & Gevaert, O. (2024). Digital profiling of gene expression from histology images with linearized attention. Nature Communications, 15(1), 9886.

[4] Carrillo-Perez, F., Pizurica, M., Ozawa, M. G., Vogel, H., West, R. B., Kong, C. S., ... & Gevaert, O. (2023). Synthetic whole-slide image tile generation with gene expression profile-infused deep generative models. Cell Reports Methods, 3(8).

[5] Carrillo-Perez, F., Pizurica, M., Zheng, Y., Nandi, T. N., Madduri, R., Shen, J., & Gevaert, O. (2025). Generation of synthetic whole-slide image tiles of tumours from RNA-sequencing data via cascaded diffusion models. Nature Biomedical Engineering, 9(3), 320-332.

[6] Xiang, J., Li, M., Hou, S., Chen, Y., Luo, X., Ji, Y., ... & Li, R. (2026). A Generative Foundation Model for Multimodal Histopathology. arXiv preprint arXiv:2604.03635.

[7] Jaume, G., Oldenburg, L., Vaidya, A., Chen, R. J., Williamson, D. F., Peeters, T., ... & Mahmood, F. (2024). Transcriptomics-guided slide representation learning in computational pathology. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (pp. 9632-9644).

[8] Xu, Y., Wang, Y., Zhou, F., Ma, J., Jin, C., Yang, S., ... & Chen, H. (2025). A multimodal knowledge-enhanced whole-slide pathology foundation model. Nature Communications.

[9] Chen, W., Zhang, P., Tran, T. N., Xiao, Y., Li, S., Shah, V. V., ... & Wang, G. (2025). A visual–omics foundation model to bridge histopathology with spatial transcriptomics. Nature Methods, 22(7), 1568-1582.

[10] Lin, Y., Luo, L., Chen, Y., Zhang, X., Wang, Z., Yang, W., ... & Yu, R. (2024). St-align: A multimodal foundation model for image-gene alignment in spatial transcriptomics. arXiv preprint arXiv:2411.16793.

[11] Huang, T., Liu, T., Babadi, M., Ying, R., & Jin, W. (2025). STPath: a generative foundation model for integrating spatial transcriptomics and whole-slide images. NPJ Digital Medicine, 8(1), 659.

[12] Yang, C., Li, H., Wu, Y., Zhang, Y., Jiao, Y., Zhang, Y., ... & Gao, X. (2025). Past: A multimodal single-cell foundation model for histopathology and spatial transcriptomics in cancer. arXiv preprint arXiv:2507.06418.

[13] Lee, Y., Liu, X., Hao, M., Liu, T., & Regev, A. (2024). PathOmCLIP: Connecting tumor histology with spatial gene expression via locally enhanced contrastive learning of Pathology and Single-cell foundation model. bioRxiv, 2024-12.

[14] Jaume, G., Doucet, P., Song, A. H., Lu, M. Y., Almagro-Pérez, C., Wagner, S. J., ... & Mahmood, F. (2024). Hest-1k: A dataset for spatial transcriptomics and histology image analysis. Advances in Neural Information Processing Systems, 37, 53798-53833.

[15] Chen, J., Zhou, M., Wu, W., Zhang, J., Li, Y., & Li, D. (2024). Stimage-1k4m: A histopathology image-gene expression dataset for spatial transcriptomics. Advances in neural information processing systems, 37, 35796-35823.

[16] Ahlmann-Eltze, C., Huber, W., & Anders, S. (2025). Deep-learning-based gene perturbation effect prediction does not yet outperform simple linear baselines. Nature Methods, 22(8), 1657-1661.
+++
title = "Med-CrossViT: A WSI-RNA-Seq fusion model"
date = "2025-05-02"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

+++

I while back I had the idea of using cross-attention for implementing an early fusion model of my two modalities of choice: digital pathology and transcriptomics. I decided to go for it, since it was not available, and called it [Med-CrossViT](https://github.com/pacocp/Med-CrossViT). Instead of using two augmentations of the same image as input, this model uses two different modalities: bags digital pathology tiles features and RNA-Seq.

An example of the original architecture can be seen below:
[Image of the original architecture](https://github.com/pacocp/blog/blob/main/assets/crossvit.png)

Instead of having two different branches (one for small patches and one for big patches), here we have two different modality heads. For digital pathology, a bag of tiles is used where a representation is obtained using a foundation model, and each patch is a token. For gene expression, the gene expression from a set of genes is projected using an embedder, that tokenize the expression, add positional encodings, and include a CLS token. Then, two transformer encoders are used (one per modality) and a cross-attention layer is used. Finally, two MLPs are used.

I also implemented a dataset that reads the tile embeddings from an h5 file, and the gene expression from an csv file. All the code is documented using [lazydocs](https://github.com/ml-tooling/lazydocs), and can be build using the associated Makefile. Here is a snippet for building the model and running it with some random inputs:

```python
from med_crossvit.med_crossvit import MedCrossViT

v = MedCrossViT(
    num_classes=2,
    depth=4,
    wsi_dim=768,
    rna_dim=768,
    wsi_num_tiles=50,
    wsi_enc_depth=2,
    wsi_enc_heads=8,
    wsi_enc_mlp_dim=2048,
    wsi_enc_dim_head=64,
    rna_enc_depth=2,
    rna_enc_heads=8,
    rna_enc_mlp_dim=2048,
    rna_enc_dim_head=64,
    rna_num_genes=100,
    cross_atnn_depth=2,
    cross_attn_heads=8,
    cross_attn_dim_head=64,
    dropout=0.1,
    emb_dropout=0.1,
    return_attn=True)

# get a lot of this
wsi_bag = torch.rand((16, 50, 768))
rna_seq = torch.rand((16, 100))

# train for multiple steps
pred, attn = v(wsi_bag, rna_seq)
print(pred)
print(pred.shape)
print(attn[0].shape) # WSI CLS token
print(attn[1].shape) # RNA CLS tokenize
```

This implementation is heavily based in the Cross ViT implementation by [lucidrains](https://github.com/lucidrains). Thanks for all the awesome work!

I hope someone can test it and see if it is better than other approaches!





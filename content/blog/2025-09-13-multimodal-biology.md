+++
title = "The future of AI for biology needs to be multimodal, and that means we
need to get or data right."
date = "2025-09-13"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

+++

The use of AI is becoming more and more widespread in biology, with significant progress and a noticeable increase in research papers in specific areas. For example, digital pathology has been completely transformed by AI models. One of the first papers I read in this field, published in Nature Medicine back in 2018 by [Coudray et. al.](https://www.nature.com/articles/s41591-018-0177-5), was a landmark achievement that showed what AI was capable of at the time. While it wasn't the very first paper on AI in digital pathology ([Ertosun et al.](https://pubmed.ncbi.nlm.nih.gov/26958289/). published a paper in 2015, for instance), it was a sensation because it demonstrated that a tumor's mutational status could be predicted from inexpensive and widely available digital pathology slides. This was during the beginning of my PhD, and it was a revelation: data plus computing power equals powerful models.

This pattern has since been repeated in other biological fields, such as radiology, genomics, and proteomics. While these areas are distinct, they all share a common thread: they are modalities where a clear pattern can be learned, and more importantly, vast amounts of data have been collected over many years.

The use of self-supervised models has proven incredibly effective for learning general patterns that can be applied to a wide range of downstream tasks, including [treatment response prediction](https://www.nature.com/articles/s41698-024-00765-w), [prognosis prediction](https://www.nature.com/articles/s41698-024-00731-6), and even [full transcriptomics prediction](https://www.nature.com/articles/s41467-024-54182-5). This is truly incredible, and much of the credit goes to Meta, as the majority of foundation models for digital pathology are based on the DINOv2 architecture.

### The Challenge of Multimodal Data

However, there's a crucial piece we're still missing. In many cases, we are using a single modality to predict a clinical outcome. Although features are often combined and used to train a predictive model, there are very few instances where this combined representation is learned during the initial pre-training phase.

For omics and imaging data, the [THREADS](https://arxiv.org/abs/2501.16652) model is a great example. It was trained in a contrastive manner using whole-slide images (WSI) along with transcriptomics and genomics data. This approach provides a more comprehensive representation of the patient, integrating information from different biological scales. However, this model was trained on only about 50,000 examples—a far cry from the 200 million data points used for models like [UNI2](https://huggingface.co/MahmoodLab/UNI2-h) or the one million slides used for [H-Optimus-1](https://huggingface.co/bioptimus/H-optimus-1).

When I think about these models and the data used to train them, I'm reminded of Richard Sutton's ["The Bitter Lesson"](http://www.incompleteideas.net/IncIdeas/BitterLesson.html). The core idea is that given enough data and computing power, general-purpose models will always outperform specialized, hand-engineered ones. Based on this premise, and the fact that we haven't yet fully exploited all the available data in biology (unlike in the language space, as a [beautiful post from Obviously Wrong]((https://obviouslywrong.substack.com/p/the-bitter-lesson-is-misunderstood)) points out), we should have a huge opportunity for progress, right?

I don't think it's that simple. Unlike other fields (perhaps with the exception of robotics), obtaining multimodal biological data is extremely difficult. It must have taken a tremendous effort for the Mahmood lab to gather their data, even with existing resources like TCGA and GTEX. It's nearly impossible to have all relevant modalities for a single patient.

Therefore, before we can even think about building the next generation of multimodal foundation models—which will unlock incredible capabilities in the bio-AI space—we first need to get our data infrastructure right. We need robust database systems that can provide easy, federated access to multimodal data. We also need dedicated efforts to build the large, diverse biological databases that will truly enable the training of large-scale multimodal models.

I'm excited about the future, but we have to roll up our sleeves and do the hard work of collecting, organizing, and publicly sharing the data first.


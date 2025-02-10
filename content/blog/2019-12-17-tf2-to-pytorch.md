+++
title = "Moving from Tensorflow 2.0 to Pytorch"
date = "2019-12-17"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

+++

I was having many issues with GPU memory management in a Transfer Learning problem when using Tensorflow 2.0 with Keras module. Even though I was using Inception V3 model with all the weights of the architecture freezed, the code ran out of memory. The weights of the model occupied only 98 MB therefore that wasn't the problem. Input shape was (299, 299, 3) so not huge images were being used, but I couldn't use a batch size higher than 16.

After three days trying to work around this issue, I decided to port the code to Pytorch. And this is where the magic happends. I had zero problems when executing the code and with memory management. Everything worked smoothly.

There's no way back.


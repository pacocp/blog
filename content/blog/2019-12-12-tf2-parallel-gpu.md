+++
title = "Parallel training in Tensorflow 2.0 "
date = "2019-12-17"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

+++
Recently I have been able to use 2 GPUs for model training. I was having problems while training with both of them (only one of them was being used).

At the end, thanks to this [issue thread](https://github.com/tensorflow/tensorflow/issues/30321), I knew that with the new eager mode, there exist a bug for multi gpu training.

In order to fix it, you need to disable eager mode using the following command:

```python
import tensorflow as tf
tf.compat.v1.disable_eager_execution()
```

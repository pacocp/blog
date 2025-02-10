+++
title = "Combine images vertically"
date = "2019-12-17"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

+++

Recently I needed to combine a bunch of images for a book. Since I didn't want to use a software I thought it would be easier to combine them using Python with the following script (thanks to [Python Programming](https://pythonprogramming.altervista.org/join-all-images-vertically-or-horizontally/?doing_wp_cron=1576619477.2015759944915771484375)):

```python
import numpy as np
from PIL import Image
import glob

list_im = glob.glob('*.png')
imgs = [ Image.open(i) for i in list_im ]
# choose the minimum size and resize the other ones
min_shape = sorted( [(np.sum(i.size), i.size ) for i in imgs])[0][1]
imgs_comb = np.vstack( (np.asarray( i.resize(min_shape) ) for i in imgs ) )
imgs_comb = Image.fromarray( imgs_comb)
imgs_comb.save( 'imagename.png')
```
+++
title = "Using glob for file retrieval"
date = "2019-12-12T09:13:54+01:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = [python]
+++

# When is *glob* useful?

Usually I need to iterate over lots of CSV files. Imaging that you have loads of CSV files which contains the EMG (declared as an array of floats) information of different patients. You want to read them with Python's library [Pandas](https://pandas.pydata.org/). Having to declare each CSV  by hand and then pass it to Pandas would be utterly painfull. Insted, there exist an amazing Python library called [glob](https://docs.python.org/3.6/library/glob.html) that helps with this task.

# Examples of *glob* usage

## Using glob for file retrieval within a folder

Following the example that has been explained before, imaging I want to retrieve all filenames for looping them and apply some operations after getting a Pandas Dataframe. This would be extremely easy with glob:

```python
import pandas as pd
import glob

for filename in glob.glob('path/to/folder/*.csv'):
    data = pd.read_csv(filename)
    # some operations here...
```

Making use of regular expressions, any kind of filename can be retrieved. For instance:

```python
import glob

glob.glob('./[0-9].*')
glob.glob('*izq*.csv')
```

## Using *glob* recursively

*glob* can also me used recursively using the **recursive** parameter. An example of its use:

```python
import glob

for filename in glob.glob('**/*.csv', recursive=True):
    data = pd.read_csv(filename)
    # some operations here...
```


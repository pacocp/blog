+++
title = "How to set Python3 as default version on MacOS"
date = "2020-01-10"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

+++

In order not to have to type everytime you want to run Python 3
```
python3
```
I wanted to link to the default version of Python. Then you need to use the following commands:

Install again Python 3 with brew:
```
brew install python
```

Look how it is installed:
```
ls -l /usr/local/bin/python*
```

Different versions should appear. We would create a new default python symlink to the version we want to use:

```
ln -s -f /user/local/bin/python3.7 /usr/local/bin/python
```

Exit the terminal. Now, if we type 
```
python --version
```

we should see the corresponding version of Python we have linked.

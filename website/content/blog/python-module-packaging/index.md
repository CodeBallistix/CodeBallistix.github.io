---
title: "Understanding Python Module Packaging: A Hands-on Guide to.whl File Packaging"
date: 2023-11-28
draft: false
summary: "To allow straightforward distribution, you must pack your Python modules into.whl (Wheel) files. To make it simple and easy for you to package and distribute your Python modules, we'll guide you through the process in this post."
description: "To allow straightforward distribution, you must pack your Python modules into.whl (Wheel) files. To make it simple and easy for you to package and distribute your Python modules, we'll guide you through the process in this post."
tags: [python, packaging, wheel, whl, py, python packaging, pypi, pypi.org, pip, twine]
showAuthor: true
authors:
  - mohit
showAuthorBadges: true
---
{{< lead >}}
Python is flexible due to its large module library, and working with other developers on your
inventions is essential. To allow straightforward distribution, you must pack your Python
modules into.whl (Wheel) files. To make it simple and easy for you to package and distribute
your Python modules, we'll guide you through the process in this post.
{{< /lead >}}

## What are .whl Files?

> <cite>Wheels are the new standard of Python distribution and are intended to replace eggs. Support is offered in pip >= 1.4 and setuptools >= 0.8.[^1]</cite>
[^1]: <https://pythonwheels.com/>

A Wheel, or `.whl` file, is a binary distribution format designed specifically for use with
Python applications. Wheels, as contrast to conventional source distributions, come with
precompiled code, which makes deployments quicker and more effective.

## How to Create Wheel Files?
### **Step 1**: Setup the Environment
Make sure you have the necessary tools installed before you start packing. Execute the
subsequent command:
```bash
pip install wheel setuptools
```


### **Step 2**: Create setup.py

Make a setup.py file in your project's root directory to include package metadata. Observe
this simple example:

```python3
from setuptools import setup, find_packages

setup(
    name='your_package_name',
    version='0.1.0',
    packages=find_packages(),
    install_requires=[
        # List your dependencies here
    ],
)
```

### Optional Stuff in `setup.py`

Here are some additional options you can include in the `setup()` function:

1. Packages
    ```python3
    packages=['your_package_name', 'your_package_name.submodule']
    ```
2. Package_data
    ```python3
    package_data={'your_package_name': ['data/*.csv', 'config/*.ini'],}
    ```
3. Include_package_data:
    ```python3
    include_package_data=True
    ```
4. Zip_safe:
    ```python3
    zip_safe=False
    ```
5. Python_requires:
    ```python3
    python_requires='>=3.6'
    ```
6. Author and Author's Email:
    ```python3
    author='YourName',
    author_email='your.email@example.com'
    ```
7. License:
    ```python3
    license='MIT'
    ```
8. Packages that are required to be installed for your package to work correctly:
    ```python3
    install_requires=[
        'dependency1>=1.0',
        'dependency2==2.3.4',
    ]
    ```

### **Step 3**: Create __init__.py file

Create a folder and create a `__init__.py` file in that folder and write your class, module which
you want to access when you install your `.whl` file.

### **Step 4**: Construct the Wheel Distribution

Run the following command to create the distribution of wheels:
```bash
python setup.py sdist bdist_wheel
```

After running the above code we will get these two files in dist Folder:
```
dist
|-- demopack-0.1.tar.gz
|-- demopack-0.1-py3-none-any.whk
```

### **Step 5**: Distribute Your Box

Give people access to the.whl file you produced. Installing it may be done with:
```bash
pip install your_package_name-0.1.0-py3-none-any.whl
```

### **Step 6**: If desired, upload to PyPI (Optional)

You might think about submitting your package to the Python Package Index (PyPI) for wider
distribution. After installing twine, do the below commands:
```bash
pip install twine
twine upload dist/* 
```

You may need to create an account on pypi for this before.

## Conclusion
Being able to package your Python modules into.whl files improves your ability to contribute
to the open-source Python community. Wheels provide a smooth integration experience for
your code into a variety of applications with faster installations, broad compatibility, and
easier dependency management. Take pride in your packaging, spread the word about your
inventions, and contribute significantly to the growing Python community
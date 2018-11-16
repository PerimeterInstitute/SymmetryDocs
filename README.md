## Welcome to the Symmetry cluster's documentation


### Modules

### Slurm

### Python

#### "System" python

The "system" versions of python, `/usr/bin/python` and `/usr/bin/python3`, are provided by the Ubuntu distribution.
We will only update these versions as new versions are released by Ubuntu, and we will probably only install
python packages that also have an Ubuntu package (for example, `python-numpy`).

You can install your own python packages using `pip install --user`.  For example:

```
pip3 install --user astropy
```

This will install the `astropy` package inside my home directory, in `$HOME/.local/lib/python3.7/site-packages/astropy`.

#### Conda

We have installed the `anaconda` python distribution.  You can use it via:

```
module load anaconda2
```

or

```
module load anaconda3
```

If you prefer to have more control over your python environment (or you want to have multiple independent setups), 
you can use the `conda` package manager.  For example:

```
module load miniconda3
conda create -n myenvA astropy
source activate myenvA
python -c "import astropy; print(astropy.__file__)"
###> /home/me/.conda/envs/myenvA/lib/python3.7/site-packages/astropy/__init__.py
```





### (markdown cheat sheet)

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

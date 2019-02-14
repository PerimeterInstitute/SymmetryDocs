# Python

#### Local versions

We have installed Conda distributions of Python with some popular python
packages pre-installed.  If you require additional packages, please
contact the [help desk](mailto:help@perimeterinstitute.ca) and we will
either install the package, or advise how best to proceed.

To use these python environments, run one of these commands:
```
module load python
```
for the default Python 3.7 version, or

```
module load python/3.6
```

or

```
module load python/2.7
```

for these older versions.  Once you have loaded the module, you should see
that this is the `python` command that will be run:

```
$ module load python
$ which python
/cm/shared/apps/conda-environments/python37/bin/python
$ python --version
Python 3.7.0
```

If you would like to install a new package that is available through
`pip`, you can install it within your home directory by running:

```
pip install --user [package-name]
```

Note that these python environments are the same ones used in the
Jupyterhub service, so if you install a local package this way, it
will be available to you in Jupyterhub.


#### "System" Python

The "system" versions of Python, `/usr/bin/python` and `/usr/bin/python3`, are provided by the Ubuntu distribution.
We will only update these versions as new versions are released by Ubuntu, and we will probably only install
Python packages that also have an Ubuntu package (for example, `python-numpy`).
We would recommend using the Local versions, as described above, or Anaconda if you want to create independent environments.

#### Conda Python

We have also installed the `anaconda` Python distribution, which is a full-featured python distribution.
You can use it via:

```
module load anaconda2
```

or

```
module load anaconda3
```

If you prefer to have more control over your Python environment (or you want to have multiple independent setups), 
you can use the `conda` package manager to build an environment.  For example:

```
module load miniconda3
# Create a new environment named "myenvA", and install 'astropy' into it.
conda create -n myenvA astropy
# "Activate" (use) that environment
source activate myenvA
# Now observe that if you run python, it can find the 'astropy' you installed in your environment.
python -c "import astropy; print(astropy.__file__)"
###> /home/me/.conda/envs/myenvA/lib/python3.7/site-packages/astropy/__init__.py
# when you are finished:
source deactivate
```



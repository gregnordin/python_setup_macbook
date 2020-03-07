# Purpose

Document how I have set up python on my MacBook Pro. 


# Motivation

Several years ago I created a base Anaconda installation with the most up to date python version at that time, 3.6. For a time I added packages as needed to this environment to explore various topics. I also created conda environments to get more recent python versions. For the last year I have begun to follow the standard practice of creating specific non-conda environments for each of my projects using `python -m venv .venv --prompt="<project name>"`. My base anaconda installation has become so old and crusty that I can't update it to a more recent python version--the update always fails. Rather than just add more conda environments with later python versions to my old Anaconda base installation, I want to have a setup that allows me to easily update and change any part of it as my needs change and as new python versions become available.


# Approach

Set up a base Miniconda environment and then create conda environments for particular purposes. Some of these conda environments will be used only as the base environment for creating normal `venv` environments. Conda environments include the following:

- Base miniconda environment
  - Bare root environment that will be easy to update to future versions of python as needed. I don't plan to use it for anything other than as a base to create conda environments because such conda environments are very easy to create and delete as needed and don't affect the root environment.
- `anaconda_py37` - Anaconda 2019-10 environment
  - This will be my main python environment for everyday use. 
  - I will not install any extra python packages here. I can therefore update Anaconda as needed, or delete the whole thing and create a new anaconda environment without affecting any other environments.
  - This will also be the launching point for everyday jupyter lab use.
- `py37`
  - Minimal environment to use with `python -m venv .venv --prompt="<project name>"` to create specific Python 3.7 environments for various projects.
- `py38`
  - Minimal environment to use with `python -m venv .venv --prompt="<project name>"` to create specific Python 3.8 environments for various projects.
- Other conda environments as needed


# Installation

## Delete old set up

### Anaconda

    $ rm -rf ~/anaconda3
    $ rm -rf ~/.conda
    $ rm -rf ~/.anaconda
    $ rm -rf ~/.condarc 
    $ rm -rf ~/.condamanager/
    $ rm -rf ~/.jupyter/
    $ rm -rf ~/.spyder*

### Jupyter

    $ rm -rf ~/.jupyter/
    $ rm -rf ~/Library/Jupyter

## Install miniconda

Download installer `Miniconda3 MacOSX 64-bit pkg` from [Miniconda](https://docs.conda.io/en/latest/miniconda.html). Double click on downloaded file and go through installation process.

The miniconda python version is installed at `~/opt/miniconda3`. Close and open terminal window, or run `$ source ~/.bash_profile` to activate new miniconda python installation. This should put `~/opt/miniconda3/bin` at the beginning of the PATH variable (`$ echo $PATH` to check). Now we get the following:

    (base)
    $ which python
    /Users/nordin/opt/miniconda3/bin/python
    (base)
    $ python --version
    Python 3.7.4
    (base)
    $ which conda
    /Users/nordin/opt/miniconda3/bin/conda
    (base)
    $ conda --version
    conda 4.7.12

## Update conda

    $ conda update -n base -c defaults conda
    (base)
    $ conda --version
    conda 4.8.2

## Install anaconda environment

    $ conda create --name anaconda_py37 anaconda
        ... <lots of stuff> ...
        #
        # To activate this environment, use
        #
        #     $ conda activate anaconda_py37
        #
        # To deactivate an active environment, use
        #
        #     $ conda deactivate

### Check that jupyterlab works

    (base)
    $ conda activate anaconda_py37
    (anaconda_py37)
    $ jupyter lab

Seems to work. Executing `!which python` in the notebook returns `/Users/nordin/opt/miniconda3/envs/anaconda_py37/bin/python`

## Install python 3.7 environment

    (base)
    $ conda create --name py37 python=3.7

## Install python 3.8 environment

    (base)
    $ conda create --name py38 python=3.8


# Create non-conda project-specific virtual environments with `venv`

See https://github.com/gregnordin/starter_project_files.

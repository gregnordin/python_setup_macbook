# Purpose

My purpose here is to document how I have set up python on my MacBook Pro. 


# Approach

My approach is to set up a base Miniconda environment and then create conda environments for particular purposes. Environments include the following:

- Base miniconda environment
  - Bare root environment that will be easy to update to future version of python as needed. I don't plan to use it for anything other than as a base to create conda environments because they are very easy to create and delete as needed.
- Anaconda 2019-10 environment
  - This will be my main python environment for everyday use. I will not install any extra python packages here.
  - This will also be the launching point for everyday jupyter lab use.
- py37
  - Minimal environment to use with project specific `python -m venv .venv --prompt="project name" for Python 3.7 projects
- py38
  - Minimal environment to use with project specific `python -m venv .venv --prompt="project name" for Python 3.8 projects
- Other environments as needed


# Motivation

I used to have a base Anaconda installation and would create conda environments and project-specific non-conda environments. After a few years my base anaconda installation became so old and crusty that I couldn't update it to a more recent version because the update would fail. If I start with a bare miniconda base installation and don't use it for anything I should be able to avoid this problem, particularly because it is quite easy to create and delete new conda and non-conda environments.


# Installation

## Make sure old anaconda is deleted (if installed)

    $ rm -rf ~/anaconda3
    $ rm -rf ~/.conda
    $ rm -rf ~/.anaconda
    $ rm -rf ~/.condarc 
    $ rm -rf ~/.condamanager/
    $ rm -rf ~/.jupyter/
    $ rm -rf ~/.spyder*

## Delete old jupyter installation

    $ rm -rf ~/.jupyter/

Also delete `~/Library/Jupyter`.

## Miniconda

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

## Anaconda environment

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

## Python 3.7

    (base)
    $ conda create --name py37 python=3.7

## Python 3.8

    (base)
    $ conda create --name py38 python=3.8


# Creating non-conda project-specific virtual environments

See https://github.com/gregnordin/starter_project_files.

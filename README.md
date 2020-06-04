# Purpose

Document how I set up python on my MacBook Pro. 


# Motivation

Several years ago I created a base Anaconda installation with the current python version, Python 3.6. For a time I added packages as needed to this environment as I did various projects. I also created conda environments to get more recent python versions. 

For the last year I have begun to follow the standard python best practice (see, for example, [A quick-and-dirty guide on how to install packages for Python](https://snarky.ca/a-quick-and-dirty-guide-on-how-to-install-packages-for-python/) by Brett Cannon, or the [Creating Virtual Environments](https://packaging.python.org/tutorials/installing-packages/#creating-virtual-environments) at python.org) of creating specific non-conda environments for each of my projects using `python -m venv .venv --prompt="<project name>"`. In the meantime, my base anaconda installation has become so old and crusty that I can't update it to a more recent python version--the update always fails. Rather than just add more conda environments with later python versions to my old Anaconda base installation, I want to have a setup that allows me to easily update and change any part of it as my needs change and as new python versions become available. I also want to set up JupyterLab so that I don't have a bunch of extraneous kernels floating around cluttering things up, while still having some virtual environments available as specific kernels.  


# Approach

Set up a base Miniconda environment and create conda environments for particular purposes. Some of these conda environments will be used only as the base environment for creating normal `venv` environments. Conda environments include the following:

- Base miniconda environment
  - Bare root environment that will be easy to update to future versions of python as needed. I don't plan to use it for anything other than as a base to create conda environments. Such conda environments are very easy to create and delete as needed and don't affect the root environment. Likewise, the base miniconda environment is so minimal that it should be easily updateable.
- `jupyter_py37` 
    - Minimal environment from which other virtual environment kernels are available.
    - This will be the launching point for everyday JupyterLab or Jupyter notebook use.
- `anaconda_py37` 
    - Anaconda 2019-10 environment
    - This will be my main python environment for everyday use. 
    - I will not install any extra python packages here. I can therefore update Anaconda as needed, or delete the whole thing and create a new anaconda environment without affecting other environments.
- `py37`
    - Minimal environment to use with `python -m venv .venv --prompt="<project name>"` to create specific Python 3.7 environments for various projects.
- `py38`
    - Minimal environment to use with `python -m venv .venv --prompt="<project name>"` to create specific Python 3.8 environments for various projects.


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
    $ rm -rf /usr/local/share/jupyter/kernels/*

Note: to see where jupyter keeps its config and other information do the following:

    $ jupyter --paths
    config:
        /Users/nordin/.jupyter
        /Users/nordin/opt/miniconda3/envs/anaconda_py37/etc/jupyter
        /usr/local/etc/jupyter
        /etc/jupyter
    data:
        /Users/nordin/Library/Jupyter
        /Users/nordin/opt/miniconda3/envs/anaconda_py37/share/jupyter
        /usr/local/share/jupyter
        /usr/share/jupyter
    runtime:
        /Users/nordin/Library/Jupyter/runtime


## Install miniconda

Download installer `Miniconda3 MacOSX 64-bit pkg` from [Miniconda](https://docs.conda.io/en/latest/miniconda.html). Double click on downloaded file and go through installation process.

The miniconda python version is installed at `~/opt/miniconda3`. The PATH has been modified in `~/.bash_profile` but still needs to be activated in any open terminal windows. Either close terminal window and open a new one, or run `$ source ~/.bash_profile` to update PATH so that the new miniconda python installation becomes the default. You can check by running `echo $PATH`, in which case `~/opt/miniconda3/bin` should be the first item. Now we get the following:

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

## Create environment for jupyter & add extensions I like

    (base)
    $ conda create -n jupyter_py37 python=3.7
    (base)
    $ conda activate jupyter_py37
    (jupyter_py37)
    $ pip install jupyterlab
    (jupyter_py37)

    # Double check that the kernel has been installed
    $ jupyter kernelspec list
    Available kernels:
        python3    /Users/nordin/opt/miniconda3/envs/jupyter_py37/share/jupyter/kernels/python3
    (jupyter_py37)

    # I find TOC is a must-have jupyter lab extension
    $ jupyter labextension install @jupyterlab/toc
    (jupyter_py37)

    # In case I use jupyter notebook, install nice pack of extensions
    $ pip install jupyter_contrib_nbextensions
    (jupyter_py37)
    $ jupyter contrib nbextension install --sys-prefix
    
    # Also install https://github.com/matplotlib/ipympl
    conda install -c conda-forge ipympl
    # Notebook
    conda install -c conda-forge widgetsnbextension
    # JupyterLab
    # conda install nodejs
    jupyter labextension install @jupyter-widgets/jupyterlab-manager
    jupyter labextension install jupyter-matplotlib

## Create anaconda environment

    # Install environment
    (base)
    $ conda create --name anaconda_py37 anaconda

    # Activate environment
    (base)
    $ conda activate anaconda_py37

    # Install jupyterlab
    (anaconda_py37)
    $ pip install jupyterlab

    # Make the kernel available in `jupyter_py37` environment
    (anaconda_py37)
    $ python -m ipykernel install --prefix=/Users/nordin/opt/miniconda3/envs/jupyter_py37 --name 'anaconda_py37'

    # Switch to the `jupyter_py37` environment
    (anaconda_py37)
    $ conda activate jupyter_py37

    # Double check that the anaconda_py37 kernel is available
    (jupyter_py37)
    $ jupyter kernelspec list
    Available kernels:
      anaconda_py37    /Users/nordin/opt/miniconda3/envs/jupyter_py37/share/jupyter/kernels/anaconda_py37
      python3          /Users/nordin/opt/miniconda3/envs/jupyter_py37/share/jupyter/kernels/python3

    # Open jupyterlab and make sure the kernel is available
    (jupyter_py37)
    $ jupyter lab
    # Yes, it works

## Create python 3.7 environment

    (base)
    $ conda create --name py37 python=3.7

## Create python 3.8 environment

    (base)
    $ conda create --name py38 python=3.8


# Create non-conda project-specific virtual environments with `venv`

See `https://github.com/gregnordin/starter_project_files`.

## Refresh virtual environment in existing projects

    $ cd <existing project directory>

    # If using old terminal window:
    $ source ~/.bash_profile

    # Delete old virtual environment
    $ rm -rf .venv

    # Create new virtual environment based on conda `py37` environment
    $ conda activate py37
    $ python -m venv .venv --prompt="<prompt text>"
    $ conda deactivate

    # Activate new virtual environment
    $ source .venv/bin/activate

    # Install packages
    (<prompt text>)
    $ pip install -e .


# Jupyter

## Objective

For each non-conda virtual environment I create, I often want to be able to start jupyterlab from within it and have the virtual environment python be the only python available. For some non-conda virtual environments, I want them to also be available from within my main conda jupyter environment.

## Information

See [Install kernel for different environments](https://ipython.readthedocs.io/en/latest/install/kernel_install.html#kernels-for-different-environments).

## Install

From within the non-conda virtual environment:

    (name_of_virtual_environment) (base)
    $ pip install jupyterlab
    (name_of_virtual_environment) (base)
    $ jupyter kernelspec list
    Available kernels:
        python3    /Users/nordin/Documents/Projects/.../.venv/share/jupyter/kernels/python3

Jupyterlab can now be run from this virtual environment, and its python is the only python available from within jupyterlab. 

If I also want this virtual environment's python available in my conda jupyter environment do the following:

    (name_of_virtual_environment) (base)
    $ python -m ipykernel install --prefix=/Users/nordin/opt/miniconda3/envs/jupyter_py37 --name 'name_of_virtual_environment'

This will throw a warning, but it can be ignored. Going to the `jupyter_py37` environment:

    (jupyter_py37)
    $ jupyter kernelspec list
    Available kernels:
      python3              /Users/nordin/opt/miniconda3/envs/jupyter_py37/share/jupyter/kernels/python3
      name_of_virtual_environment    /Users/nordin/opt/miniconda3/envs/jupyter_py37/share/jupyter/kernels/name_of_virtual_environment

## Set default browser for jupyterlab/notebook

Use the following instructions to set a different browser as default than your system default browser. For example, my system default browser is Brave, but when I execute `jupyter lab` or `jupyter notebook` I want jupyter to automatically open in Opera.

### Create `jupyter_notebook_config.py`

    (base)
    $ conda activate jupyter_py37
    (jupyter_py37)
	$ jupyter notebook --generate-config
    
    
### Edit `jupyter_notebook_config.py`

Open `~/.jupyter/jupyter_notebook_config.py` in editor. 

Under this line `# c.NotebookApp.browser = ''` add the following:

	import webbrowser
    webbrowser.register('opera', None, webbrowser.MacOSXOSAScript('/Applications/Opera.app'))
    c.NotebookApp.browser = 'opera'


**Now `$ jupyter notebook` opens the initial notebook window in Opera!**

**Critical part**: since I'm on Mac OS I must use `webbrowser.MacOSXOSAScript('/Applications/Opera.app')` instead of `webbrowser.GenericBrowser('/Applications/Opera.app')` as directed in many non-mac instructions found online.


## Usage

### Start jupyterlab from within environment (regular)

Activate environment: `conda activate jupyter_py37` OR `source .venv/bin/activate` from within correct directory

    # Start jupyterlab
    (<text for prompt>)
    $ jupyter lab

### Start jupyterlab from within environment (background)

Activate environment: `conda activate jupyter_py37` OR `source .venv/bin/activate` from within correct directory

    # Start jupyterlab in the background so the terminal remains free to use
    (<text for prompt>)
    $ nohup jupyter lab &

    # Kill jupyterlab that's running in the background
    $ jupyter notebook list
        Currently running servers:
        http://localhost:8888/?token=blah :: /Users/nordin/Documents/Projects/Python/190709_starter_project_files
    # Note the port jupyter is running on (8888). Use this to find the PID of the jupyter process
    $ lsof -n -i4TCP:8888
        COMMAND     PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
        python3.7 38870 nordin    8u  IPv4 0xb27f89b37e23f69b      0t0  TCP 127.0.0.1:ddi-tcp-1 (LISTEN)
    # Now kill the jupyter process
    $ kill 38870

### Remove jupyter kernel

    $ jupyter kernelspec uninstall <kernel_to_uninstall>

### Use TOC with jupyterlab

    (<text for prompt>)
    $ jupyter labextension install @jupyterlab/toc

### Use jupyter_contrib_nbextensions with jupyter notebooks

    Only works with `jupyter notebook`, not 'jupyter lab`

    (<text for prompt>)
    $ pip install jupyter_contrib_nbextensions
    (<text for prompt>)
    $ jupyter contrib nbextension install --sys-prefix

In the last command, `--sys-prefix` is the critical part to get the extensions installed locally in the virtual environment.


# Alternatives

Use pyenv and pipx with venv. See [Modern Python Developer's Tookit - Pyenv](https://pycon.switowski.com/02-packages/pyenv/) and [Modern Python Developer's Tookit - pipx](https://pycon.switowski.com/02-packages/pipx/) by Sebastian Witowski. His Pycon 2020 presentation: [Tutorial: Sebastian Witowski - Modern Python Developer's Toolkit - Pycon 2020](https://www.youtube.com/watch?v=WkUBx3g2QfQ).

Another older (2017) approach based on pyenv and with a focus on jupyter: [The definitive guide to setup my Python workspace](https://medium.com/@henriquebastos/the-definitive-guide-to-setup-my-python-workspace-628d68552e14).
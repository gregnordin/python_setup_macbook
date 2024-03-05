# Purpose

Document how I set up python on my MacBook Pro.

Updated 1/15/24.

# Motivation

In early 2022 when I transferred my files and projects from a 2018 MacBook Pro (Intel i7) to a new 2021 M1 Max MacBook Pro (Apple Silicon), my miniconda installation came with it, which means my base environment is still for Intel processors. I've had problems lately with installing Apple Silicon-specific python packages so I am deleting miniconda and my python environments and starting from scratch. 

# Approach

I'm following [Managing Python installations in 2023](https://alisterburt.com/blog/2023/05/27/managing-python-installations-in-2023/) and using [`micromamba`](https://mamba.readthedocs.io/en/latest/user_guide/micromamba.html), a tiny version of the `mamba` package manager. It is a C++ executable and therefore does not have or need a base python environment. 

I will create micromamba environments for particular purposes, and readily delete them when no longer needed. Some of micromamba environments will be used only as the base environment for creating normal `venv` environments with packages installed with `pip`. See [Pip vs Conda: an in-depth comparison of Python’s two packaging systems](https://pythonspeed.com/articles/conda-vs-pip/) for a good comparison of pip vs conda.

# Delete old miniconda tool and python environments

See [Uninstall Miniconda on MacOS](https://stackoverflow.com/questions/60131438/uninstall-miniconda-on-macos).

```
$ rm -rf ~/.condarc ~/.conda ~/.continuum
$ sudo rm -rf ~/opt/miniconda3
```

In `.bash_profile`, delete starting at `# added by Miniconda3 4.7.12 installer` through line `# <<< conda init <<<`.

Delete python environments in consolidated location, `~/python_envs`:

```
$ ls -l python_envs/
    total 0
    drwxr-xr-x  3 nordin  staff   96 Jan 10  2022 panel_env
    drwxr-xr-x  3 nordin  staff   96 Oct 23  2021 panel_voila_opencv
    drwxr-xr-x  8 nordin  staff  256 Jul 23  2022 spectra_tools
    drwxr-xr-x  5 nordin  staff  160 Jan 21  2021 voila_opencv

$ rm -rf ~/python_envs
```

# Install and setup `micromamba`

Use `curl` installation method at [Micromamba installation](https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html).

```
$ "${SHELL}" <(curl -L micro.mamba.pm/install.sh)
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
    0     0    0     0    0     0      0      0 --:--:--  0:00:01 --:--:--     0
    100  3069  100  3069    0     0   1913      0  0:00:01  0:00:01 --:--:--     0
    Micromamba binary folder? [~/.local/bin]
    Init shell (bash)? [Y/n] Y
    Configure conda-forge? [Y/n] Y
    Prefix location? [~/micromamba]
    Modifying RC file "/Users/nordin/.bash_profile"
    Generating config for root prefix "/Users/nordin/micromamba"
    Setting mamba executable to: "/Users/nordin/.local/bin/micromamba"
    Adding (or replacing) the following in your "/Users/nordin/.bash_profile" file

    # >>> mamba initialize >>>
    # !! Contents within this block are managed by 'mamba init' !!
    export MAMBA_EXE='/Users/nordin/.local/bin/micromamba';
    export MAMBA_ROOT_PREFIX='/Users/nordin/micromamba';
    __mamba_setup="$("$MAMBA_EXE" shell hook --shell bash --root-prefix "$MAMBA_ROOT_PREFIX" 2> /dev/null)"
    if [ $? -eq 0 ]; then
        eval "$__mamba_setup"
    else
        alias micromamba="$MAMBA_EXE"  # Fallback on help from mamba activate
    fi
    unset __mamba_setup
    # <<< mamba initialize <<<

    Please restart your shell to activate micromamba or run the following:\n
    source ~/.bashrc (or ~/.zshrc, ~/.xonshrc, ~/.config/fish/config.fish, ...)
```

Add `alias conda="micromamba"` to `.bash_profile`.

Open new `bash` shell in `Warp` terminal and confirm micromamba version, channels, platform, etc.:

```
$ micromamba --version
    1.5.6
# Make sure that conda alias works
$ conda --version
    1.5.6

# Get info
$ conda info

       libmamba version : 1.5.6
     micromamba version : 1.5.6
           curl version : libcurl/8.5.0 SecureTransport (OpenSSL/3.2.0) zlib/1.2.13 zstd/1.5.5 libssh2/1.11.0 nghttp2/1.58.0
     libarchive version : libarchive 3.7.2 zlib/1.2.13 bz2lib/1.0.8 libzstd/1.5.5
       envs directories : /Users/nordin/micromamba/envs
          package cache : /Users/nordin/micromamba/pkgs
                          /Users/nordin/.mamba/pkgs
            environment : None (not found)
           env location : -
      user config files : /Users/nordin/.mambarc
 populated config files : /Users/nordin/.condarc
       virtual packages : __unix=0=0
                          __osx=13.3.1=0
                          __archspec=1=arm64
               channels : https://conda.anaconda.org/conda-forge/osx-arm64
                          https://conda.anaconda.org/conda-forge/noarch
                          https://conda.anaconda.org/nodefaults/osx-arm64
                          https://conda.anaconda.org/nodefaults/noarch
       base environment : /Users/nordin/micromamba
               platform : osx-arm64

# Check and see if there is a more recent version
$ micromamba self-update
    nodefaults/osx-arm64 (check zst)                    Checked  0.3s
    nodefaults/noarch (check zst)                       Checked  0.4s
    nodefaults/noarch                                  116.0 B @ 875.0 B/s  0.1s
    nodefaults/osx-arm64                               125.0 B @ 819.0 B/s  0.2s
    conda-forge/osx-arm64                                8.1MB @   5.7MB/s  1.4s
    conda-forge/noarch                                  13.2MB @   6.7MB/s  2.0s

    Your micromamba version (1.5.6) is already up to date.
```

# Create python environments

## `default`

```
$ micromamba create -n default python
    conda-forge/osx-arm64                                       Using cache
    conda-forge/noarch                                          Using cache
    nodefaults/osx-arm64                                          No change
    nodefaults/noarch                                             No change

    Transaction

    Prefix: /Users/nordin/micromamba/envs/default

    Updating specs:

    - python


    Package               Version  Build               Channel          Size
    ────────────────────────────────────────────────────────────────────────────
    Install:
    ────────────────────────────────────────────────────────────────────────────

    + xz                    5.2.6  h57fd34a_0          conda-forge     236kB
    + libexpat              2.5.0  hb7217d7_1          conda-forge      63kB
    + ncurses                 6.4  h463b476_2          conda-forge     795kB
    + bzip2                 1.0.8  h93a5062_5          conda-forge     122kB
    + libffi                3.4.2  h3422bc3_5          conda-forge      39kB
    + libzlib              1.2.13  h53f4e23_5          conda-forge      48kB
    + ca-certificates  2023.11.17  hf0a4a13_0          conda-forge     154kB
    + readline                8.2  h92ec313_1          conda-forge     250kB
    + tk                   8.6.13  h5083fa2_1          conda-forge       3MB
    + libsqlite            3.44.2  h091b4b1_0          conda-forge     815kB
    + openssl               3.2.0  h0d3ecfb_1          conda-forge       3MB
    + tzdata                2023d  h0c530f3_0          conda-forge     120kB
    + python               3.12.1  hdf0ec26_1_cpython  conda-forge      13MB
    + wheel                0.42.0  pyhd8ed1ab_0        conda-forge      58kB
    + setuptools           69.0.3  pyhd8ed1ab_0        conda-forge     471kB
    + pip                  23.3.2  pyhd8ed1ab_0        conda-forge       1MB

    Summary:

    Install: 16 packages

    Total download: 24MB

    ────────────────────────────────────────────────────────────────────────────


    Confirm changes: [Y/n] y

    Transaction starting
    libexpat                                            63.4kB @ 643.0kB/s  0.1s
    libffi                                              39.0kB @ 323.3kB/s  0.1s
    bzip2                                              122.3kB @ 670.9kB/s  0.2s
    xz                                                 235.7kB @   1.1MB/s  0.2s
    wheel                                               57.6kB @ 251.3kB/s  0.1s
    ncurses                                            794.7kB @   2.6MB/s  0.3s
    libzlib                                             48.1kB @ 150.5kB/s  0.1s
    ca-certificates                                    154.4kB @ 366.7kB/s  0.1s
    tzdata                                             119.6kB @ 280.9kB/s  0.1s
    readline                                           250.4kB @ 400.6kB/s  0.2s
    libsqlite                                          815.3kB @ 843.4kB/s  0.3s
    openssl                                              2.9MB @   2.9MB/s  0.9s
    tk                                                   3.1MB @   2.9MB/s  0.9s
    setuptools                                         470.5kB @ 414.7kB/s  0.2s
    pip                                                  1.4MB @   1.2MB/s  0.8s
    python                                              13.1MB @   6.0MB/s  1.9s
    Linking xz-5.2.6-h57fd34a_0
    Linking libexpat-2.5.0-hb7217d7_1
    Linking ncurses-6.4-h463b476_2
    Linking bzip2-1.0.8-h93a5062_5
    Linking libffi-3.4.2-h3422bc3_5
    Linking libzlib-1.2.13-h53f4e23_5
    Linking ca-certificates-2023.11.17-hf0a4a13_0
    Linking readline-8.2-h92ec313_1
    Linking tk-8.6.13-h5083fa2_1
    Linking libsqlite-3.44.2-h091b4b1_0
    Linking openssl-3.2.0-h0d3ecfb_1
    Linking tzdata-2023d-h0c530f3_0
    Linking python-3.12.1-hdf0ec26_1_cpython
    Linking wheel-0.42.0-pyhd8ed1ab_0
    Linking setuptools-69.0.3-pyhd8ed1ab_0
    Linking pip-23.3.2-pyhd8ed1ab_0

    Transaction finished

    To activate this environment, use:

        micromamba activate default

    Or to execute a single command in this environment, use:

        micromamba run -n default mycommand

$ micromamba activate default
$ conda env list
  Name     Active  Path
──────────────────────────────────────────────────────────
  base             /Users/nordin/micromamba
  default  *       /Users/nordin/micromamba/envs/default
$ micromamba env list
  Name     Active  Path
──────────────────────────────────────────────────────────
  base             /Users/nordin/micromamba
  default  *       /Users/nordin/micromamba/envs/default
$ which python
    /Users/nordin/micromamba/envs/default/bin/python
$ python --version
    Python 3.12.1
$ python -c "import platform; print(platform.processor())"
    arm
```

# Create new virtual environment and add as a Jupyter kernel

```
$ micromamba activate default
# Be sure you are in the directory in which you want to locate the virtual environment directory
$ cd <desired_directory>
$ python -m venv venv
$ source venv/bin/activate
$ pip install ipykernel
$ python -m ipykernel install --user --name <env_name> --display-name="<env_name>"
$ jupyter kernelspec list
    Available kernels:
        <env_name>
        ...
```

# Jupyter

## Delete old Jupyter stuff so I can start with a clean slate

    $ rm -rf ~/.jupyter/
    $ rm -rf ~/Library/Jupyter
    $ rm -rf /usr/local/share/jupyter/kernels/*

# New Jupyter conda environment

```
micromamba create -n jupyter_2401 jupyterlab
```


---
# OLD

# Motivation

Several years ago I created a base Anaconda installation with the current python version, Python 3.6. For a time I added packages as needed to this environment as I did various projects. I also created conda environments to get more recent python versions.

For the last year I have begun to follow the standard python best practice (see, for example, [A quick-and-dirty guide on how to install packages for Python](https://snarky.ca/a-quick-and-dirty-guide-on-how-to-install-packages-for-python/) by Brett Cannon, or [Creating Virtual Environments](https://packaging.python.org/tutorials/installing-packages/#creating-virtual-environments) at python.org) of creating specific non-conda environments for each of my projects using `python -m venv .venv --prompt="<project name>"`. In the meantime, my base anaconda installation has become so old and crusty that I can't update it to a more recent python version--the update always fails. Rather than just add more conda environments with later python versions to my old Anaconda base installation, I want to have a setup that allows me to easily update and change any part of it as my needs change and as new python versions become available. I also want to set up JupyterLab so that I don't have a bunch of extraneous kernels floating around cluttering things up, while still having some virtual environments available as specific kernels.

# Approach

Set up a base Miniconda environment and create conda environments for particular purposes. Some of these conda environments will be used only as the base environment for creating normal `venv` environments with packages installed with `pip`. See [Pip vs Conda: an in-depth comparison of Python’s two packaging systems](https://pythonspeed.com/articles/conda-vs-pip/) for a good comparison of pip vs conda. My conda environments include the following:

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
    $ python -m ipykernel install --user --name=name_of_virtual_environment

Note that the kernel is put in ~/Library/Jupyter/kernels, which makes it available to any instance of Jupyter when you start jupyterlab or jupyter notebook. Going to the `jupyter_py37` environment:

    (jupyter_py37)
    $ jupyter kernelspec list
    Available kernels:
      python3              /Users/nordin/opt/miniconda3/envs/jupyter_py37/share/jupyter/kernels/python3
      name_of_virtual_environment    /Users/nordin/Library/Jupyter/kernels/name_of_virtual_environment

If you should wish to remove the environment from Jupyter, activate a jupyter environment and execute the following:

    (jupyter_py37)
    $ jupyter kernelspec uninstall name_of_virtual_environment

This will remove the virtual environment as a kernel for any instance of Jupyter (except if you invoke jupyter from within the virtual environment itself).

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

Use pyenv and pipx with venv. See Sebastian Witowski's Pycon 2020 presentation: [Tutorial: Sebastian Witowski - Modern Python Developer's Toolkit - Pycon 2020](https://www.youtube.com/watch?v=WkUBx3g2QfQ) and [accompanying website with text and explanations](https://pycon-switowski.netlify.app/). See also [Managing Multiple Python Versions With pyenv](https://realpython.com/intro-to-pyenv) and [How to Set Up a Python Project For Automation and Collaboration](https://eugeneyan.com/writing/setting-up-python-project-for-automation-and-collaboration/).

Another older (2017) approach based on pyenv and with a focus on jupyter: [The definitive guide to setup my Python workspace](https://medium.com/@henriquebastos/the-definitive-guide-to-setup-my-python-workspace-628d68552e14).

# 12/24/20

## Create jupyter environment with python 3.8, jupyterlab 3.0, jupyterlab_widgets

    (base)
    $ conda env list
    # conda environments:
    #
    base                  *  /Users/nordin/opt/miniconda3
    anaconda_py37            /Users/nordin/opt/miniconda3/envs/anaconda_py37
    jupyter_py37             /Users/nordin/opt/miniconda3/envs/jupyter_py37
    py37                     /Users/nordin/opt/miniconda3/envs/py37
    py38                     /Users/nordin/opt/miniconda3/envs/py38
    (base)
    $ conda create -n jupyter_py38 python=3.8
    (base)
    $ conda activate jupyter_py38
    (jupyter_py38)
    $ pip install jupyterlab
    $ jupyter lab --version
    3.0.0
    (jupyter_py38)
    # See https://ipywidgets.readthedocs.io/en/stable/user_install.html
    $ pip install ipywidgets
    (jupyter_py38)
    $ jupyter --version
    jupyter core     : 4.7.0
    jupyter-notebook : 6.1.6
    qtconsole        : not installed
    ipython          : 7.19.0
    ipykernel        : 5.4.2
    jupyter client   : 6.1.7
    jupyter lab      : 3.0.0
    nbconvert        : 6.0.7
    ipywidgets       : 7.6.0
    nbformat         : 5.0.8
    traitlets        : 5.0.5
    (jupyter_py38)
    $ conda env list
    # conda environments:
    #
    base                     /Users/nordin/opt/miniconda3
    anaconda_py37            /Users/nordin/opt/miniconda3/envs/anaconda_py37
    jupyter_py37             /Users/nordin/opt/miniconda3/envs/jupyter_py37
    jupyter_py38          *  /Users/nordin/opt/miniconda3/envs/jupyter_py38
    py37                     /Users/nordin/opt/miniconda3/envs/py37
    py38                     /Users/nordin/opt/miniconda3/envs/py38
    
## Create anaconda python 3.8 environment & add to jupyter_py38 environment

    (base)
    $ conda create --name anaconda_py38 anaconda
    (base)
    $ conda activate anaconda_py38
    (anaconda_py38)
    $ python -m ipykernel install --prefix=/Users/nordin/opt/miniconda3/envs/jupyter_py38 --name 'anaconda_py38'
    [InstallIPythonKernelSpecApp] WARNING | Installing to /Users/nordin/opt/miniconda3/envs/jupyter_py38/share/jupyter/kernels, which is not in ['/Users/nordin/Library/Jupyter/kernels', '/Users/nordin/opt/miniconda3/envs/anaconda_py38/share/jupyter/kernels', '/usr/local/share/jupyter/kernels', '/usr/share/jupyter/kernels', '/Users/nordin/.ipython/kernels']. The kernelspec may not be found.
    Installed kernelspec anaconda_py38 in /Users/nordin/opt/miniconda3/envs/jupyter_py38/share/jupyter/kernels/anaconda_py38
    (base)
    $ conda activate jupyter_py38
    (jupyter_py38)
    $ pwd
    /Users/nordin
    (jupyter_py38)
    $ jupyter lab
    # It seems to work fine and has TOC enabled by default
    
    # Install extensions for jupyter notebook
    (jupyter_py38)
    $ pip install jupyter_contrib_nbextensions
    (jupyter_py38)
    $ jupyter contrib nbextension install --sys-prefix
    # Tried it and works great - TOC is enabled
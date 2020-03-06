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

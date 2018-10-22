# Conda env manager commands
  Install from here https://conda.io/miniconda.html
  
   - Cheat sheet https://conda.io/docs/_downloads/conda-cheatsheet.pdf
   - Here is how to manage conda environments https://conda.io/docs/user-guide/tasks/manage-environments.html
   - `conda env list`
   - `conda create --name myenv`
   - `source activate myenv`
   - `conda search ...`
   - `conda install ...`
   - `conda list --export > env.txt`
   - `conda install --file env.txt`
   - `source deactivate`
   - `conda env export > environment.yml` Alternative way to save env through yml
   - `conda env create --name <env> -f environment.yml`
   - `conda env update -f environment.yml`

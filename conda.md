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
   - `conda env export > env.yml` Alternative way to save env through yml
   - `conda env create --name <env> -f env.yml`
   - `conda env update -f env.yml`

If you want an env to be accessible in Jupyter
  ```bash
  (base)$ source activate myenv
  (myenv)$ conda install ipykernel
  # Then restart your Jupyter Notebook/Lab
  ```
  

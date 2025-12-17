# Data-Science-MLOps


- First we create an Enviroment for our project.
  ```bash
  conda create -p venv python==3.10
  ```
- If Conda is not installed, here are the steps to install it. Otherwise move onto the next step.
  ```bash
  wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
  bash Miniconda3-latest-Linux-x86_64.sh
  source ~/.bashrc                                                                    # reload shell
  conda --version
  ```

- Activate the created environment.
  ```bash
  conda activate venv/
  ```
  
- Create requirements.txt.
  ```bash
  pandas 
  mlflow
  notebook
  numpy
  scikit-learn
  matplotlib
  python-box
  pyYAML
  tqdm
  ensure
  joblib
  types-PyYAML
  Flask
  Flask-Cors
  ```

- Install all the requirements.
  ```bash
  pip install -r requirements.txt
  ```

- We need to have a generic project structure (which is used in the industry). There are different libraries that are available in python so we'll create a "template.py" in which we will add an automated script written in python and that automated script will
  help in creating the entire project structure. Basically it will contain all the files that will be required for this project and in general for Data-Science-MLOps projects. </br>
  For eg: "____init.py____" file will be created so that any functions, libraries or classes we're calling inside this can be referenced anywhere. This will be considered as a package. We will be able to call any libraries, packages because of this contructor.
```bash
import os
from pathlib import Path
import logging

project_name="DataScienceMLops"

list_of_files=[
    ".github/workflows/.gitkeep",
    f"src/{project_name}/__init__.py",
    f"src/{project_name}/components/__init__.py",
    f"src/{project_name}/utils/__init__.py",
    f"src/{project_name}/utils/__common__.py",
    f"src/{project_name}/config/__init__.py",
    f"src/{project_name}/config/configuration.py",
    f"src/{project_name}/pipeline/__init__.py",
    f"src/{project_name}/entity/__init__.py",
    f"src/{project_name}/entity/config_entity.py",
    f"src/{project_name}/constants/__init__.py",
    "config/config.yaml",
    "params.yaml",
    "schema.yaml",
    "main.py",
    "Dockerfile",
    "setup.py",
    "research/research.ipynb",
    "templates/index.html"

]
```


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
We'll create a for loop in which every file we're gonna split it into file-name along with file-path. We'll use a condition that if the file directory is not empty, then we make direcotry using "os.makedirs()" (this works in both Mac or Linux irrespective of the OS). Another condition will be used to check if the filepath does not exist, then we'll open that specific filepath in the write mode and then we're going to create the file. And as soon as we run template.py, entire project structure will be created.
```bash
for filepath in list_of_files:
    filepath=Path(filepath)
    filedir, filename=os.path.split(filepath)

    if filedir!="":
        os.makedirs(filedir,exist_ok=True)
        logging.info(f"Creating directory {filedir} for the file : {filename}")

    if (not os.path.exists(filepath)) or (os.path.getsize(filepath) == 0):
        with open(filepath,"w") as f:

            pass
            logging.info(f"Creating empty file: {filepath}")

    else:
        logging.info(f"{filename} already exists")
```
After this we can commit & push the entire file structure.

- setup.py is used if we want to create our entire project as a package. And then we can put it into the pipeline.
- We'll put this code in src/DataScienceMLops/__init__.py file
  ```bash
  import os
  import sys
  import logging

  logging_str="[%(asctime)s: %(levelname)s: %(module)s: %(message)s:]"

  log_dir="logs"
  log_filepath=os.path.join(log_dir,"logging.log")
  os.makedirs(log_dir,exist_ok=True)

  logging.basicConfig(
    level=logging.INFO,
    format=logging_str,

    handlers=[
        logging.FileHandler(log_filepath), 
        logging.StreamHandler(sys.stdout)
    ]
  )

  logger=logging.getLogger("datasciencelogger")
  ```
  And this code in main.py:
  ```bash
  from src.DataScienceMLops import logger

  logger.info("Welcome to our custom logging data science")
  ```
  Then when we run main.py, we get something like this: "  [2025-12-17 14:35:29,543: INFO: main: Welcome to our custom logging data science:]  "

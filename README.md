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
  

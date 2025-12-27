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

- Explanation for ensure_annotations function which is inside utils/common.py. ensure_annotations: It is a decorator whose sole purpose is to enforce runtime type checking based on function annotations (like whatever datatype is expected, only that should be passed.)
    ```bash
    # example of ensure_annotations usage
    @ensure_annotations
    def save_json(path: Path, data: dict):
      """save json data

      Args:
        path (Path): path to json file
        data (dict): data to be saved in json file
      """
      with open(path, "w") as f:
        json.dump(data, f, indent=4)

      logger.info(f"json file saved at: {path}")
    ```
    
- Workflow for ML Pipeline: We'll see how we can design each and every specific modeule.
  1. __Data Ingestion__ First we import the modules like "os" module which is used to interact with the Operating System like if we want to change directory etc. Then we will add the configurations for Data Ingestion step in config.yaml This config needs to be passed to Data Ingestion pipeline like        root_dir, source_dir, local_data_file, unzip_dir etc.  </br>
     Inside src/DataScienceMLops/constants/init.py, we will define the config, params & schema path. Everything will be defined in the form of constants. This then will be called in ConfigurationManager class in init function. Before we're defining the ConfigurationManager function, above it we will need to Import      these constants using  </br>
     ```bash
     from src.DataScienceMLops.constants import *
     from src.DataScienceMLops.utils.common import read_yaml, create_directories
     ```
     </br>
     __Steps for Data Ingestion:__ </br>
     
     </br>
     - Update config.yaml : config.yaml consists of configurations that we require. </br>
     - Update schema.yaml : schema.yaml is used both in data ingestion & validation. In data Validation, we check the schema of the input we're getting. </br>
     - Update params.yaml : For parameters </br>
     - Update the entity </br>
     - Update the configuration manager in src config: Whenever the configuration manager is loaded, whatever is there in config, schema & params.yaml, it should be loaded.  </br>
     - Update the components </br>
     - Update the pipeline </br>
     - Update the main.py </br>
     </br>
     </br>
  3. Data Validation: We'll validate each and every feature that we have and alongwith that we'll use those yaml files that we have specifically defined. Inside Artifacts folder, just like data_ingestion, we'll create out data_validation folder. Then we'll have the unzipped data directory, this will       be the input so that we'll be able to compare all the features from this dataset and this particular input is available in data ingestion. Then there will be status_file in which we'll update the status whether the validation is True or False. All this will be added in config.yml alongwith            data_ingestion & other stages.
     ```bash
     data_validation:
      root_dir: artifacts/data_validation
      unzip_dir: artifacts/data_ingestion/winequality-red.csv
      STATUS_FILE: artifacts/data_validation/status.txt
     ```
     All the features will be added to schema.yml with the help of pandas-read so that they can be validated. </br>
     </br>
     In data Validation, we'll also check if there are any NULL values or not. If there are some NULL values then we can handle them using techniques like Mean Amputation orwe can replace categorical features by mode and all. Then we will provide root_dir, STATUS_FILE, unzip_data_dir, all_schema to        our data validation component. Almost all the steps will be same as that of Data-Ingestion, here we will write get_data_validation_config(). </br>
     Again we'll follow the same workflow for Data Validation (that we created using project-structure script) that we did for data-ingestion.  </br>
     </br>
     - Update config.yaml : config.yaml consists of configurations that we require. </br>
     - Update schema.yaml : schema.yaml is used both in data ingestion & validation. In data Validation, we check the schema of the input we're getting. </br>
     - Update params.yaml : For parameters </br>
     - Update the entity </br>
     - Update the configuration manager in src config: Whenever the configuration manager is loaded, whatever is there in config, schema & params.yaml, it should be loaded.  </br>
     - Update the components </br>
     - Update the pipeline </br>
     - Update the main.py </br>
     </br>
     </br>
     While defining the Data_validation task, after we read the data set, we need to compare with the schema (basically the features). If the feature name is equal, then we can proceed with setting the status to TURE. A Function is created called "validate_all_columns" which has return type as             Boolean. It will read the dataset then it will take all the columns in the form of a list. Then we will assign this all_schema and then we're going to take keys from there. Then we will compare all the columns to all the columns and setting the status to False if column not in all_schema. If it       is False, then we will open the STATUS_FILE in Write mode and update the validation_status to FALSE otherwise update it to TRUE.
 ```bash
  class DataValiadtion:
    def __init__(self, config: DataValidationConfig):
        self.config = config

    def validate_all_columns(self)-> bool:
        try:
            validation_status = None

            data = pd.read_csv(self.config.unzip_data_dir)
            all_cols = list(data.columns)

            all_schema = self.config.all_schema.keys()

            
            for col in all_cols:
                if col not in all_schema:
                    validation_status = False
                    with open(self.config.STATUS_FILE, 'w') as f:
                        f.write(f"Validation status: {validation_status}")
                else:
                    validation_status = True
                    with open(self.config.STATUS_FILE, 'w') as f:
                        f.write(f"Validation status: {validation_status}")

            return validation_status
        
        except Exception as e:
            raise e
 ```

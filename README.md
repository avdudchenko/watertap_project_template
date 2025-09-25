# WaterTAP Project Template
A starting point for analysis using WaterTAP

## Starting a new project

Start by identifying what is the project you are going to work on and give it a reasonable name. 
For example, for multi stage reverse osmosis analysis a name of "multi_stage_ro_analysis" could be a good choice and will be an example used below.

To start a new project:

    (1) Download this repo as a zip folder and extract in a location where you would like to start doing analysis with WaterTAP. 
    (2) Rename the "template_name" folder in 'src' folder with name of your project so its structure is "src/your_project_name"
    (3) In project_setup.yaml replace "template_name" in the "name" field with name of your project
    (4) In pyproject.toml, change "template_name" to your project name

For example, for multi_stage_ro_analysis we would have following folder structure:

```
├──src
│   │
│   └──multi_stage_ro_analysis
│       └──__init__.py
│ 
├──project_setup.yaml
├──README.md
├──.env_example
└──requirements.txt
```

Our project_setup.yaml would look as follows:

```
    name: multi_stage_ro_analysis
    channels: 
    - conda-forge
    - defaults
    dependencies:
    - python=3.11*
    - git
    - pytest
    - pydotenv
    - pip
    - pip:
        - -r requirements.txt
```
Our pyproject.toml file would look as follows:
```
    [project]
    name = multi_stage_ro_analysis
    readme = "README.md"
    dynamic = ["version"]
    dependencies = [
        "watertap"
    ]
``

Once setup you can install your analysis module by opening a terminal in source directory and running following command:

    conda env create -f project_setup.yml

This will createa  new conda environmnet with name of your project, in our case multi_stage_ro_analysis, we can access it by running:

```
    conda activate your project name
```

In our multi stage reverse osmosis analysis this would be done through:

```
    conda activate multi_stage_ro_analysis
```

As we develop the repository we might want to add new packages or dependencies, this can be done either through pyproject.toml file, or through yaml file for conda installations. 
We can update our installation by simply running
``` 
    conda  update -n your project name --f project_setup.yml
```
 and in our example
```
    conda  update -n multi_stage_ro_analysis --f project_setup.yml
```

Finally, in general we do not wish to store data in this module or repository, to specify location for where data should be stored we can use installed dotenv module. 

In the source directory simply create a .env file and in side it add a key and location for where data should be loaded from or stored. 

For example, if our data is should be saved on a cloud service located in D:\Cloud_Service\multi_stage_reverse_osmosis_results

We would add 
ANALYSIS_DATA = D:\Cloud_Service\multi_stage_reverse_osmosis_results 

into our .env file, example the .env_example file. 
NOTE: dotenv only looks for .env file on the root directory of the project, as such .env_example or .env_random_name will not be loaded.

To access the location in our code we can simply import the module and access it throug our os. 

```
    import dotenv
    import os

    dotenv.load_dotenv()
    results_location = os.getenv(ANALYSIS_DATA)
```

Finally, if you decide to upload your code to github/gitlab via git your can add .env to .gitignroe file to ensure your environmental keys are not shared with the world. This should look as 

```
*.pyc
*.env
```
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
├──.gitignore
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
```

Once setup you can install your analysis module by opening a terminal in source directory and running following command:

    conda env create -f project_setup.yml

This will createa  new conda environmnet with name of your project (YOUR_PROJECT_NAME), in our case multi_stage_ro_analysis, we can access it by running:

```
    conda activate YOUR_PROJECT_NAME
```

In our multi stage reverse osmosis analysis this would be done through:

```
    conda activate multi_stage_ro_analysis
```

As we develop the repository we might want to add new packages or dependencies, this can be done either through pyproject.toml file, or through yaml file for conda installations. 
We can update our installation by simply running
``` 
    conda  update -n YOUR_PROJECT_NAME --f project_setup.yml
```
 and in our example
```
    conda  update -n multi_stage_ro_analysis --f project_setup.yml
```
NOTE: the update command will install missing packages and update packages to new requested version, but it will not remove or reinstall packages already installed. 


Finally, we do not wish to store data in this module or repository, to specify a location for where data should be stored we can use installed dotenv module. 

In the source directory simply create a .env file and inside it add a key and location for where data should be loaded from or stored. 

For example, if our data should be saved on a cloud service located in D:\Cloud_Service\multi_stage_reverse_osmosis_results

We would add the following to our .env file:
```
ANALYSIS_DATA = D:\Cloud_Service\multi_stage_reverse_osmosis_results 
```
NOTE: dotenv only looks for .env file on the root directory of the project, as such .env_example or .env_random_name will not be loaded.
You can revere to .env_example for example implementation.

To access the location in our code we can simply import the module and access it through our os. 

```
    import dotenv
    import os

    dotenv.load_dotenv()
    results_location = os.getenv(ANALYSIS_DATA)
```

Finally, if you decide to upload your code to github/gitlab via git your can add .env to .gitignroe file to ensure your environmental keys are not shared with the world. This should look as follows in the .gitignore file. Check the .gitignore file. for an example:

```
*.pyc
*.env
```

Working with your module. 

Now when we want to work on our repo, we can create our analysis scripts, codes, modules, or models and import or inherit them from watertap. 

For example, if create a custom model in our multi_stage_ro_analysis folder called 'multi_stage_ro_model.py' which as function 'build' we can access it as any other module in python:

```
    from multi_stage_ro_analysis.multi_stage_ro_model import build
```

You should think carefully about what folders should go into your analysis module, for example are you planning to create:

    (1) custom unit models
    (2) flowsheets
    (3) plotting scripts 

If so it might make sense to create a folder for each one to house your scripts and code. so your final structure might look like this. 

```
├──src
│   │
│   └──multi_stage_ro_analysis
│       ├──custom_unit_models
│       │   └──__init__.py
│       ├──flowsheets
│       │   └──__init__.py
│       ├──plotting_scripts
│       │   └──__init__.py
│       └──__init__.py
│ 
├──project_setup.yaml
├──README.md
├──.env_example
├──.gitignore
└──requirements.txt
```

Note how we add under score instead of space to ensure files are python safe, as well as __init__.py file in each folder, that will ensure this library works as an actual python module. 
So if you want to use some of the custom units or flowsheets in a different analysis you could simply install them as a dependency through (assuming your uploaded your repo to git)

```
    pip install git+https://github.com/_your_user_name_/_your_repository_.git
```

or from local folder by 
``` 
    conda env update -n YOUR_OTHER_PROJECT --file project_setup.yml
```